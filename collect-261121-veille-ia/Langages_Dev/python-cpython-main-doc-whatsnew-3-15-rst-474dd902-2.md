---
id: collect-261121-veille-ia/langages-dev/python-cpython-main-doc-whatsnew-3-15-rst-474dd902-2
title: "Python cpython main doc whatsnew 3 15 rst"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["contributed", "code", "python", "compiler", "operations", "rubric", "more", "machine"]
source: ""
source_anchor: ""
source_lines: [1501, 2140]
section: "Langages_Dev"
---
<https://github.com/python/cpython/blob/main/Tools/jit/README.md>`__
for all supported platforms.
(Contributed by Savannah Ostrowski in :gh:`140973`.)
.. rubric:: A new tracing frontend
The JIT compiler now supports significantly more bytecode operations and
control flow than in Python 3.14, enabling speedups on a wider variety of
code. For example, simple Python object creation is now understood by the
3.15 JIT compiler. Overloaded operations and generators are also partially
supported. This was made possible by an overhauled JIT tracing frontend
that records actual execution paths through code, rather than estimating
them as the previous implementation did.
(Contributed by Ken Jin in :gh:`139109`. Support for Windows added by
Mark Shannon in :gh:`141703`.)
.. rubric:: Basic register allocation in the JIT
A basic form of register allocation has been added to the JIT compiler's
optimizer. This allows the JIT compiler to avoid certain stack operations
altogether and instead operate on registers. This allows the JIT to produce
more efficient traces by avoiding reads and writes to memory.
(Contributed by Mark Shannon in :gh:`135379`.)
.. rubric:: More JIT optimizations
More `constant-propagation <https://en.wikipedia.org/wiki/Constant_folding>`__
is now performed. This means when the JIT compiler detects that certain user
code results in constants, the code can be simplified by the JIT.
(Contributed by Ken Jin and Savannah Ostrowski in :gh:`132732`.)
:term:`Reference counts <reference count>` are avoided whenever it is safe to
do so. This generally reduces the cost of most operations in Python.
(Contributed by Ken Jin, Donghee Na, Zheao Li, Hai Zhu, Savannah Ostrowski,
Reiden Ong, Noam Cohen, Tomas Roun, PuQing, Cajetan Rodrigues, and Sacul in :gh:`134584`.)
By tracking unique references to objects, the JIT optimizer can now eliminate
reference count updates and perform in-place operations on ints and floats.
(Contributed by Reiden Ong, and Pieter Eendebak in :gh:`143414` and :gh:`146306`.)
The JIT optimizer now supports significantly more operations than in 3.14.
(Contributed by Kumar Aditya, Ken Jin, Jiahao Li, and Sacul in :gh:`131798`.)
.. rubric:: GDB and GNU ``backtrace()`` unwinding support
The JIT compiler now publishes unwind information for generated machine code to the
GDB interface on supported Linux ELF platforms. When libgcc frame
registration is available, the same unwind information is also registered for
GNU ``backtrace()`` stack walkers. This allows native debuggers, crash
handlers, and diagnostic tools using these mechanisms to unwind through JIT
frames instead of stopping at generated code.
(Contributed by Diego Russo and Pablo Galindo Salgado in :gh:`146071` and
:gh:`149104`.)
.. rubric:: Better machine code generation
The JIT compiler's machine code generator now produces better machine code
for x86-64 and AArch64 macOS and Linux targets. In general, users should
experience lower memory usage for generated machine code and more efficient
machine code versus 3.14.
(Contributed by Brandt Bucher in :gh:`136528` and :gh:`135905`.
Implementation for AArch64 contributed by Mark Shannon in :gh:`139855`.
Additional optimizations for AArch64 contributed by Mark Shannon and
Diego Russo in :gh:`140683` and :gh:`142305`.)
.. rubric:: Maintainability
The JIT optimizer's operations have been simplified.
This was made possible by a refactoring of JIT data structures.
(Contributed by Zhongtian Zheng in :gh:`148211` and Hai Zhu in :gh:`143421`.)
Removed
========
ast
---
* The constructors of :ref:`AST nodes <ast_nodes>` now raise a :exc:`TypeError`
  when a required argument is omitted or when a keyword argument that does not
  map to a field on the AST node is passed. These cases had previously raised a
  :exc:`DeprecationWarning` since Python 3.13.
  (Contributed by Brian Schubert and Jelle Zijlstra in :gh:`137600` and :gh:`105858`.)
collections.abc
---------------
* :class:`collections.abc.ByteString` has been removed from
  ``collections.abc.__all__``. :class:`!collections.abc.ByteString` has been
  deprecated since Python 3.12, and is scheduled for removal in Python 3.17.
  (Contributed by Alex Waygood in :gh:`118803`.)
ctypes
------
* Removed the undocumented function :func:`!ctypes.SetPointerType`,
  which has been deprecated since Python 3.13.
  (Contributed by Bénédikt Tran in :gh:`133866`.)
datetime
--------
* :meth:`~datetime.datetime.strptime` now raises :exc:`ValueError` when the
  format string contains ``%d`` (day of month) without a year directive.
  This has been deprecated since Python 3.13.
  (Contributed by Stan Ulbrych and Gregory P. Smith in :gh:`70647`.)
glob
----
* Removed the undocumented :func:`!glob.glob0` and :func:`!glob.glob1`
  functions, which have been deprecated since Python 3.13. Use
  :func:`glob.glob` and pass a directory to its *root_dir* argument instead.
  (Contributed by Barney Gale in :gh:`137466`.)
http.server
-----------
* Removed the :class:`!CGIHTTPRequestHandler` class
  and the ``--cgi`` flag from the :program:`python -m http.server`
  command-line interface. They were deprecated in Python 3.13.
  (Contributed by Bénédikt Tran in :gh:`133810`.)
importlib
---------
* The ``load_module()`` methods of :class:`~importlib.abc.Loader` and its
  subclasses is removed.
  The import system will no longer call it when defined on custom subclasses.
  The method has been deprecated in favor of
  :meth:`~importlib.abc.Loader.exec_module` since Python 3.4.
  (Contributed by Brett Cannon in :gh:`97850`.)
importlib.resources
-------------------
* Removed deprecated ``package`` parameter
  from :func:`importlib.resources.files` function.
  (Contributed by Semyon Moroz in :gh:`138044`.)
pathlib
-------
* Removed deprecated :meth:`!pathlib.PurePath.is_reserved`.
  Use :func:`os.path.isreserved` to detect reserved paths on Windows.
  (Contributed by Nikita Sobolev in :gh:`133875`.)
platform
--------
* Removed the :func:`!platform.java_ver` function,
  which was deprecated since Python 3.13.
  (Contributed by Alexey Makridenko in :gh:`133604`.)
sre_*
-----
* Removed :mod:`!sre_compile`, :mod:`!sre_constants` and :mod:`!sre_parse` modules.
  (Contributed by Stan Ulbrych in :gh:`135994`.)
sysconfig
---------
* Removed the *check_home* parameter of :func:`sysconfig.is_python_build`.
  (Contributed by Filipe Laíns in :gh:`92897`.)
threading
---------
* Remove support for arbitrary positional or keyword arguments in the C
  implementation of :class:`~threading.RLock` objects. This was deprecated
  in Python 3.14.
  (Contributed by Bénédikt Tran in :gh:`134087`.)
types
-----
* Removed :attr:`!codeobject.co_lnotab` from :class:`types.CodeType`,
  deprecated since Python 3.12 by :pep:`626`.
  (Contributed by Nikita Sobolev in :gh:`134690`.)
typing
------
* :class:`typing.ByteString` has been removed from ``typing.__all__``.
  :class:`!typing.ByteString` has been deprecated since Python 3.9, and is
  scheduled for removal in Python 3.17.
  (Contributed by Alex Waygood in :gh:`118803`.)
* The undocumented keyword argument syntax for creating
  :class:`~typing.NamedTuple` classes (for example,
  ``Point = NamedTuple("Point", x=int, y=int)``) is no longer supported.
  Use the class-based syntax or the functional syntax instead.
  (Contributed by Bénédikt Tran in :gh:`133817`.)
* Using ``TD = TypedDict("TD")`` or ``TD = TypedDict("TD", None)`` to
  construct a :class:`~typing.TypedDict` type with zero fields is no
  longer supported. Use ``class TD(TypedDict): pass``
  or ``TD = TypedDict("TD", {})`` instead.
  (Contributed by Bénédikt Tran in :gh:`133823`.)
* Deprecated :deco:`!typing.no_type_check_decorator` has been removed.
  (Contributed by Nikita Sobolev in :gh:`133601`.)
wave
----
* Removed the ``getmark()``, ``setmark()`` and ``getmarkers()`` methods
  of the :class:`~wave.Wave_read` and :class:`~wave.Wave_write` classes,
  which were deprecated since Python 3.13.
  (Contributed by Bénédikt Tran in :gh:`133873`.)
zipimport
---------
* Remove deprecated :meth:`!zipimport.zipimporter.load_module`.
  Use :meth:`zipimport.zipimporter.exec_module` instead.
  (Contributed by Jiahao Li in :gh:`133656`.)
Deprecated
==========
New deprecations
----------------
* :mod:`ast`:
  * Creating instances of abstract AST nodes (such as :class:`ast.AST`
    or :class:`!ast.expr`) is deprecated and will raise an error in Python 3.20.
    (Contributed by Brian Schubert in :gh:`116021`.)
* :mod:`base64`:
  * Accepting the ``+`` and ``/`` characters with an alternative alphabet in
    :func:`~base64.b64decode` and :func:`~base64.urlsafe_b64decode` is now
    deprecated.
    In future Python versions they will be errors in the strict mode and
    discarded in the non-strict mode.
    (Contributed by Serhiy Storchaka in :gh:`125346`.)
* CLI:
  * Deprecate :option:`-b` and :option:`!-bb` command-line options
    and schedule them to become no-ops in Python 3.17.
    These were primarily helpers for the Python 2 -> 3 transition.
    Starting with Python 3.17, no :exc:`BytesWarning` will be raised
    for these cases; use a type checker instead.
    (Contributed by Nikita Sobolev in :gh:`136355`.)
* :mod:`collections.abc`:
  * The following statements now cause ``DeprecationWarning``\ s to be emitted
    at runtime:
    * ``from collections.abc import ByteString``
    * ``import collections.abc; collections.abc.ByteString``.
    ``DeprecationWarning``\ s were already emitted if
    :class:`collections.abc.ByteString` was subclassed or used as the second
    argument to :func:`isinstance` or :func:`issubclass`, but warnings were not
    previously emitted if it was merely imported or accessed from the
    :mod:`!collections.abc` module.
    (Contributed by Alex Waygood in :gh:`118803`.)
* :mod:`hashlib`:
  * In hash function constructors such as :func:`~hashlib.new` or the
    direct hash-named constructors such as :func:`~hashlib.md5` and
    :func:`~hashlib.sha256`, the optional initial data parameter could
    also be passed as a keyword argument named ``data=`` or ``string=`` in
    various :mod:`hashlib` implementations.
    Support for the ``string`` keyword argument name is now deprecated and
    is slated for removal in Python 3.19. Prefer passing the initial data as
    a positional argument for maximum backwards compatibility.
    (Contributed by Bénédikt Tran in :gh:`134978`.)
* :mod:`http.cookies`:
  * :meth:`Morsel.js_output <http.cookies.Morsel.js_output>` and
    :meth:`BaseCookie.js_output <http.cookies.BaseCookie.js_output>` are
    deprecated and will be removed in Python 3.19. Use
    :meth:`Morsel.output <http.cookies.Morsel.output>` or
    :meth:`BaseCookie.output <http.cookies.BaseCookie.output>` instead.
    (Contributed by kishorhange111 in :gh:`148849`.)
* :mod:`imaplib`:
  * Altering :attr:`IMAP4.file <imaplib.IMAP4.file>` is now deprecated
    and slated for removal in Python 3.19. This property is now unused
    and changing its value does *not* explicitly close the current file.
    (Contributed by Bénédikt Tran in :gh:`142307`.)
* :mod:`profile`:
  * The :mod:`!profile` module is deprecated and will be removed in
    Python 3.17. Use :mod:`profiling.tracing` instead, which provides a
    compatible API. See :pep:`799` for details.
    (Contributed by Pablo Galindo and László Kiss Kollár in :gh:`138122`.)
* :mod:`re`:
  * :func:`re.match` and :meth:`re.Pattern.match` are now
    :term:`soft deprecated` in favor of the new :func:`re.prefixmatch` and
    :meth:`re.Pattern.prefixmatch` APIs, which have been added as alternate,
    more explicit names. These are intended to be used to alleviate confusion
    around what *match* means by following the Zen of Python's *"Explicit is
    better than implicit"* mantra. Most other language regular expression
    libraries use an API named *match* to mean what Python has always called
    *search*.
    We **do not** plan to remove the older :func:`!match` name, as it has been
    used in code for over 30 years. Code supporting older versions of Python
    should continue to use :func:`!match`, while new code should prefer
    :func:`!prefixmatch`. See :ref:`prefixmatch-vs-match`.
    (Contributed by Gregory P. Smith in :gh:`86519` and
    Hugo van Kemenade in :gh:`148100`.)
* :mod:`struct`:
  * Calling ``Struct.__new__()`` without a required argument is now
    deprecated and will be removed in Python 3.20.  Calling the
    :meth:`~object.__init__` method on an initialized :class:`~struct.Struct`
    object is deprecated and will be removed in Python 3.20.
    (Contributed by Sergey B Kirpichev and Serhiy Storchaka in :gh:`143715`.)
  * The ``'F'`` and ``'D'`` type codes are now :term:`soft deprecated`
    in favor of the two-letter forms ``'Zf'`` and ``'Zd'``.
    (Contributed by Sergey B Kirpichev in :gh:`121249`.)
* :mod:`typing`:
  * The following statements now cause ``DeprecationWarning``\ s to be emitted
    at runtime:
    * ``from typing import ByteString``
    * ``import typing; typing.ByteString``.
    ``DeprecationWarning``\ s were already emitted if :class:`typing.ByteString`
    was subclassed or used as the second argument to :func:`isinstance` or
    :func:`issubclass`, but warnings were not previously emitted if it was
    merely imported or accessed from the :mod:`!typing` module.
    (Contributed by Alex Waygood in :gh:`118803`.)
  * It is deprecated to call :func:`isinstance` and :func:`issubclass` checks on
    protocol classes that were not explicitly decorated with :func:`!runtime_checkable`
    but that inherit from a runtime-checkable protocol class.
    This will raise a :exc:`TypeError` in Python 3.20.
    (Contributed by Bartosz Sławecki in :gh:`132604`.)
* :mod:`webbrowser`:
  * :class:`!webbrowser.MacOSXOSAScript` is deprecated in favour of
    :class:`!webbrowser.MacOS` and scheduled for removal in Python 3.17.
    (Contributed by Jeff Lyon in :gh:`137586`.)
* ``__version__``:
  * The ``__version__``, ``version`` and ``VERSION`` attributes have been
    deprecated in these standard library modules and will be removed in
    Python 3.20. Use :py:data:`sys.version_info` instead.
    - :mod:`argparse`
    - :mod:`csv`
    - :mod:`ctypes`
    - :mod:`!ctypes.macholib`
    - :mod:`decimal` (use :data:`decimal.SPEC_VERSION` instead)
    - :mod:`http.server`
    - :mod:`imaplib`
    - :mod:`ipaddress`
    - :mod:`json`
    - :mod:`logging` (``__date__`` also deprecated)
    - :mod:`optparse`
    - :mod:`pickle`
    - :mod:`platform`
    - :mod:`re`
    - :mod:`socketserver`
    - :mod:`tabnanny`
    - :mod:`tarfile`
    - :mod:`tkinter.font`
    - :mod:`tkinter.ttk`
    - :mod:`wsgiref.simple_server`
    - :mod:`xml.etree.ElementTree`
    - :mod:`!xml.sax.expatreader`
    - :mod:`xml.sax.handler`
    - :mod:`zlib`
    (Contributed by Hugo van Kemenade and Stan Ulbrych in :gh:`76007`.)
.. Add deprecations above alphabetically, not here at the end.
.. include:: ../deprecations/pending-removal-in-3.16.rst
.. include:: ../deprecations/pending-removal-in-3.17.rst
.. include:: ../deprecations/pending-removal-in-3.18.rst
.. include:: ../deprecations/pending-removal-in-3.19.rst
.. include:: ../deprecations/pending-removal-in-3.20.rst
.. include:: ../deprecations/pending-removal-in-3.21.rst
.. include:: ../deprecations/pending-removal-in-future.rst
.. include:: ../deprecations/soft-deprecations.rst
C API changes
=============
New features
------------
* Add :c:func:`PyArg_ParseArray` and :c:func:`PyArg_ParseArrayAndKeywords`
  functions to parse arguments of functions using the :c:macro:`METH_FASTCALL`
  calling convention.
  (Contributed by Victor Stinner in :gh:`144175`.)
* Add the following functions for the new :class:`frozendict` type:
  * :c:func:`PyAnyDict_Check`
  * :c:func:`PyAnyDict_CheckExact`
  * :c:func:`PyFrozenDict_Check`
  * :c:func:`PyFrozenDict_CheckExact`
  * :c:func:`PyFrozenDict_New`
  (Contributed by Victor Stinner in :gh:`141510`.)
* Add the :pep:`757` export/import functions to the limited C API:
  * :c:func:`PyLong_GetNativeLayout`
  * :c:func:`PyLong_Export`
  * :c:func:`PyLong_FreeExport`
  * :c:func:`PyLongWriter_Create`
  * :c:func:`PyLongWriter_Finish`
  * :c:func:`PyLongWriter_Discard`
  (Contributed by Sergey B Kirpichev in :gh:`143869`.)
* Add :c:func:`PyObject_CallFinalizerFromDealloc` function to the limited C
  API.
  (Contributed by Victor Stinner in :gh:`146063`.)
* Add :c:func:`PySys_GetAttr`, :c:func:`PySys_GetAttrString`,
  :c:func:`PySys_GetOptionalAttr`, and :c:func:`PySys_GetOptionalAttrString`
  functions as replacements for :c:func:`PySys_GetObject`.
  (Contributed by Serhiy Storchaka in :gh:`108512`.)
* Add :c:func:`PyUnstable_Unicode_GET_CACHED_HASH` to get the cached hash of
  a string. See the documentation for caveats.
  (Contributed by Petr Viktorin in :gh:`131510`.)
* Add API for checking an extension module's ABI compatibility:
  :c:data:`Py_mod_abi`, :c:func:`PyABIInfo_Check`,
  and :c:macro:`PyABIInfo_VAR`.
  (Contributed by Petr Viktorin in :gh:`137210`.)
.. _whatsnew315-pybyteswriter:
* Implement :pep:`782`, the :ref:`PyBytesWriter API <pybyteswriter>`.
  Add functions:
  * :c:func:`PyBytesWriter_Create`
  * :c:func:`PyBytesWriter_Discard`
  * :c:func:`PyBytesWriter_Finish`
  * :c:func:`PyBytesWriter_FinishWithPointer`
  * :c:func:`PyBytesWriter_FinishWithSize`
  * :c:func:`PyBytesWriter_Format`
  * :c:func:`PyBytesWriter_GetData`
  * :c:func:`PyBytesWriter_GetSize`
  * :c:func:`PyBytesWriter_Grow`
  * :c:func:`PyBytesWriter_GrowAndUpdatePointer`
  * :c:func:`PyBytesWriter_Resize`
  * :c:func:`PyBytesWriter_WriteBytes`
  (Contributed by Victor Stinner in :gh:`129813`.)
* :c:type:`PyCriticalSection` and related functions are added to the Stable
  ABI.
  (Contributed by Petr Viktorin in :gh:`149225`.)
* Add a new :c:func:`PyImport_CreateModuleFromInitfunc` C API for creating
  a module from a *spec* and *initfunc*.
  (Contributed by Itamar Oren in :gh:`116146`.)
* Add :c:func:`PyTuple_FromArray` to create a :class:`tuple` from an array.
  (Contributed by Victor Stinner in :gh:`111489`.)
* Add a new module export hook,
  :c:func:`PyModExport_* <PyModExport_modulename>`.
  (Contributed by Petr Viktorin in :pep:`793` and :gh:`140550`.)
* Add functions that are guaranteed to be safe for use in
  :c:member:`~PyTypeObject.tp_traverse` handlers:
  :c:func:`PyObject_GetTypeData_DuringGC`,
  :c:func:`PyObject_GetItemData_DuringGC`,
  :c:func:`PyType_GetModuleState_DuringGC`,
  :c:func:`PyModule_GetState_DuringGC`, :c:func:`PyModule_GetToken_DuringGC`,
  :c:func:`PyType_GetBaseByToken_DuringGC`,
  :c:func:`PyType_GetModule_DuringGC`,
  :c:func:`PyType_GetModuleByToken_DuringGC`.
  (Contributed by Petr Viktorin in :gh:`145925`.)
* Add :c:func:`PyObject_Dump` to dump an object to ``stderr``.
  It should only be used for debugging.
  (Contributed by Victor Stinner in :gh:`141070`.)
* Implement :pep:`820`: ``PySlot`` -- Unified slot system for the C API.
  See :ref:`capi-slots` for documentation.
  This adds:
  * The :c:type:`PySlot` struct;
  * the :c:func:`PyType_FromSlots` function;
  * new slot IDs: :c:macro:`Py_slot_end`, :c:macro:`Py_slot_invalid`;
    :c:macro:`Py_slot_subslots`, :c:macro:`Py_tp_slots`,
    :c:macro:`Py_mod_slots`;
    :c:macro:`Py_tp_name`, :c:macro:`Py_tp_basicsize`,
    :c:macro:`Py_tp_extra_basicsize`, :c:macro:`Py_tp_itemsize`,
    :c:macro:`Py_tp_flags`, :c:macro:`Py_tp_metaclass`,
    :c:macro:`Py_tp_module`;
  * convenience macros: :c:macro:`PySlot_DATA`, :c:macro:`PySlot_FUNC`,
    :c:macro:`PySlot_SIZE`, :c:macro:`PySlot_INT64`, :c:macro:`PySlot_UINT64`,
    :c:macro:`PySlot_STATIC_DATA`, :c:macro:`PySlot_END`,
    :c:macro:`PySlot_PTR`, :c:macro:`PySlot_PTR_STATIC`.
  The :c:func:`PyModule_FromSlotsAndSpec` function and
  ``PyModExport`` :ref:`module export hook <extension-export-hook>` also
  use the new :c:type:`!PySlot` struct.
  The following functions are :term:`soft deprecated`:
  * :c:func:`PyType_FromSpec`
  * :c:func:`PyType_FromSpecWithBases`
  * :c:func:`PyType_FromModuleAndSpec`
  * :c:func:`PyType_FromMetaclass`
  * :c:func:`PyModule_FromDefAndSpec`
  * :c:func:`PyModule_FromDefAndSpec2`
  * :c:func:`PyModule_ExecDef`
  The slots :c:macro:`Py_tp_bases` and :c:macro:`Py_tp_base` are now
  equivalent: they can be set either to a single type or a tuple of types.
  The :c:macro:`Py_tp_bases` slot is preferred; the other is ignored if both
  are specified.
  (Contributed by Petr Viktorin in :gh:`149044`.)
* Add :c:func:`PyUnstable_ThreadState_SetStackProtection` and
  :c:func:`PyUnstable_ThreadState_ResetStackProtection` functions to set
  the stack protection base address and stack protection size of a Python
  thread state.
  (Contributed by Victor Stinner in :gh:`139653`.)
* Add :c:func:`PyUnstable_SetImmortal` C API function to mark objects as :term:`immortal`.
  (Contributed by Kumar Aditya in :gh:`143300`.)
* Restore private provisional ``_Py_InitializeMain()`` function removed in
  Python 3.14.
  (Contributed by Victor Stinner in :gh:`142417`.)
* Add :c:func:`PyUnstable_DumpTraceback` and
  :c:func:`PyUnstable_DumpTracebackThreads` functions to output Python
  stacktraces.
  (Contributed by Alex Malyshev in :gh:`145559`.)
Changed C APIs
--------------
* If the :c:macro:`Py_TPFLAGS_MANAGED_DICT` or :c:macro:`Py_TPFLAGS_MANAGED_WEAKREF`
  flag is set then :c:macro:`Py_TPFLAGS_HAVE_GC` must be set too.
  (Contributed by Sergey Miryanov in :gh:`134786`.)
* :c:macro:`PyDateTime_IMPORT` is now thread safe. Code that directly checks ``PyDateTimeAPI``
  for ``NULL`` should be updated to call :c:macro:`PyDateTime_IMPORT` instead.
  (Contributed by Kumar Aditya in :gh:`141563`.)
Removed C APIs
--------------
* Remove deprecated ``PyUnicode`` functions:
  * :c:func:`!PyUnicode_AsDecodedObject`:
    Use :c:func:`PyCodec_Decode` instead.
  * :c:func:`!PyUnicode_AsDecodedUnicode`:
    Use :c:func:`PyCodec_Decode` instead; note that some codecs (for example, "base64")
    may return a type other than :class:`str`, such as :class:`bytes`.
  * :c:func:`!PyUnicode_AsEncodedObject`:
    Use :c:func:`PyCodec_Encode` instead.
  * :c:func:`!PyUnicode_AsEncodedUnicode`:
    Use :c:func:`PyCodec_Encode` instead; note that some codecs (for example, "base64")
    may return a type other than :class:`bytes`, such as :class:`str`.
  (Contributed by Stan Ulbrych in :gh:`133612`.)
* :c:func:`!PyImport_ImportModuleNoBlock`: deprecated alias
  of :c:func:`PyImport_ImportModule`.
  (Contributed by Bénédikt Tran in :gh:`133644`.)
* :c:func:`!PyWeakref_GetObject` and :c:macro:`!PyWeakref_GET_OBJECT`:
  use :c:func:`PyWeakref_GetRef` instead. The |pythoncapi_compat_project|
  can be used to get :c:func:`!PyWeakref_GetRef` on Python 3.12 and older.
  (Contributed by Bénédikt Tran in :gh:`133644`.)
* Remove deprecated :c:func:`!PySys_ResetWarnOptions`.
  Clear :data:`sys.warnoptions` and :data:`!warnings.filters` instead.
  (Contributed by Nikita Sobolev in :gh:`138886`.)
The following functions are removed in favor of :c:func:`PyConfig_Get`.
The |pythoncapi_compat_project| can be used to get :c:func:`!PyConfig_Get`
on Python 3.13 and older.
* Python initialization functions:
  * :c:func:`!Py_GetExecPrefix`:
    use :c:func:`PyConfig_Get("base_exec_prefix") <PyConfig_Get>`
    (:data:`sys.base_exec_prefix`) instead.
    Use :c:func:`PyConfig_Get("exec_prefix") <PyConfig_Get>`
    (:data:`sys.exec_prefix`) if :ref:`virtual environments <venv-def>`
    need to be handled.
  * :c:func:`!Py_GetPath`:
    use :c:func:`PyConfig_Get("module_search_paths") <PyConfig_Get>`
    (:data:`sys.path`) instead.
  * :c:func:`!Py_GetPrefix`:
    use :c:func:`PyConfig_Get("base_prefix") <PyConfig_Get>`
    (:data:`sys.base_prefix`) instead.
    Use :c:func:`PyConfig_Get("prefix") <PyConfig_Get>`
    (:data:`sys.prefix`) if :ref:`virtual environments <venv-def>`
    need to be handled.
  * :c:func:`!Py_GetProgramFullPath`:
    use :c:func:`PyConfig_Get("executable") <PyConfig_Get>`
    (:data:`sys.executable`) instead.
  * :c:func:`!Py_GetProgramName`:
    use :c:func:`PyConfig_Get("executable") <PyConfig_Get>`
    (:data:`sys.executable`) instead.
  * :c:func:`!Py_GetPythonHome`:
    use :c:func:`PyConfig_Get("home") <PyConfig_Get>` or the
    :envvar:`PYTHONHOME` environment variable instead.
  (Contributed by Bénédikt Tran in :gh:`133644`.)
.. |pythoncapi_compat_project| replace:: |pythoncapi_compat_project_link|_
.. |pythoncapi_compat_project_link| replace:: pythoncapi-compat project
.. _pythoncapi_compat_project_link: https://github.com/python/pythoncapi-compat
Deprecated C APIs
-----------------
* Deprecate :pep:`456` support for providing an external definition
  of the string hashing scheme. Removal is scheduled for Python 3.19.
  Previously, embedders could define :c:macro:`Py_HASH_ALGORITHM` to be
  ``Py_HASH_EXTERNAL`` to indicate that the hashing scheme was provided
  externally but this feature was undocumented, untested and most likely
  unused.
  (Contributed by Bénédikt Tran in :gh:`141226`.)
* For unsigned integer formats in :c:func:`PyArg_ParseTuple`,
  accepting Python integers with value that is larger than the maximal value
  for the C type or less than the minimal value for the corresponding
  signed integer type of the same size is now deprecated.
  (Contributed by Serhiy Storchaka in :gh:`132629`.)
* :c:func:`PyBytes_FromStringAndSize(NULL, len) <PyBytes_FromStringAndSize>`
  and :c:func:`_PyBytes_Resize` are :term:`soft deprecated`,
  use the :c:type:`PyBytesWriter` API instead.
  (Contributed by Victor Stinner in :gh:`129813`.)
* :c:func:`!_PyObject_CallMethodId`, :c:func:`!_PyObject_GetAttrId` and
  :c:func:`!_PyUnicode_FromId` are deprecated since Python 3.15 and will be removed in
  Python 3.20. Instead, use :c:func:`PyUnicode_InternFromString()` and cache the result in
  the module state, then call :c:func:`PyObject_CallMethod` or
  :c:func:`PyObject_GetAttr`.
  (Contributed by Victor Stinner in :gh:`141049`.)
* Deprecate :c:member:`~PyComplexObject.cval` field of the
  :c:type:`PyComplexObject` type.
  Use :c:func:`PyComplex_AsCComplex` and :c:func:`PyComplex_FromCComplex`
  to convert a Python complex number to/from the C :c:type:`Py_complex`
  representation.
  (Contributed by Sergey B Kirpichev in :gh:`128813`.)
* Functions :c:func:`_Py_c_sum`, :c:func:`_Py_c_diff`, :c:func:`_Py_c_neg`,
  :c:func:`_Py_c_prod`, :c:func:`_Py_c_quot`, :c:func:`_Py_c_pow` and
  :c:func:`_Py_c_abs` are :term:`soft deprecated`.
  (Contributed by Sergey B Kirpichev in :gh:`128813`.)
* :c:member:`~PyConfig.bytes_warning` is deprecated
  since Python 3.15 and will be removed in Python 3.17.
  (Contributed by Nikita Sobolev in :gh:`136355`.)
* :c:macro:`!Py_INFINITY` macro is :term:`soft deprecated`,
  use the C11 standard ``<math.h>`` :c:macro:`!INFINITY` instead.
  (Contributed by Sergey B Kirpichev in :gh:`141004`.)
* The following macros are :term:`soft deprecated`:
  - :c:macro:`Py_ALIGNED`: Prefer ``alignas`` instead.
  - :c:macro:`PY_FORMAT_SIZE_T`: Use ``"z"`` directly.
  - :c:macro:`Py_LL` and :c:macro:`Py_ULL`:
    Use standard suffixes, ``LL`` and ``ULL``.
  - :c:macro:`PY_LONG_LONG`, :c:macro:`PY_LLONG_MIN`, :c:macro:`PY_LLONG_MAX`,
    :c:macro:`PY_ULLONG_MAX`, :c:macro:`PY_INT32_T`, :c:macro:`PY_UINT32_T`,
    :c:macro:`PY_INT64_T`, :c:macro:`PY_UINT64_T`, :c:macro:`PY_SIZE_MAX`:
    Use C99 types/limits.
  - :c:macro:`Py_UNICODE_SIZE`: Use ``sizeof(wchar_t)`` directly.
  - :c:macro:`Py_VA_COPY`: Use ``va_copy`` directly.
  The macro :c:macro:`Py_UNICODE_WIDE`, which was scheduled for removal,
  is :term:`soft deprecated` instead.
  (Contributed by Petr Viktorin in :gh:`146175`.)
* :c:macro:`!Py_MATH_El` and :c:macro:`!Py_MATH_PIl` are deprecated
  since Python 3.15 and will be removed in Python 3.20.
  (Contributed by Sergey B Kirpichev in :gh:`141004`.)
.. Add C API deprecations above alphabetically, not here at the end.
Build changes
=============
* Removed implicit fallback to the bundled copy of the ``libmpdec`` library.
  Now this should be explicitly enabled with :option:`!--with-system-libmpdec`
  set to ``no`` or with :option:`!--without-system-libmpdec`.
  (Contributed by Sergey B Kirpichev in :gh:`115119`.)
* The new configure option :option:`--with-missing-stdlib-config=FILE` allows
  distributors to pass a `JSON <https://www.json.org/json-en.html>`_
  configuration file containing custom error messages for :term:`standard library`
  modules that are missing or packaged separately.
  (Contributed by Stan Ulbrych and Petr Viktorin in :gh:`139707`.)
* The new configure option :option:`--with-pymalloc-hugepages` enables huge
  page support for :ref:`pymalloc <pymalloc>` arenas. When enabled, arena size
  increases to 2 MiB and allocation uses ``MAP_HUGETLB`` (Linux) or
  ``MEM_LARGE_PAGES`` (Windows) with automatic fallback to regular pages.
  On Windows, use ``build.bat --pymalloc-hugepages``.
  At runtime, huge pages must be explicitly enabled by setting the
  :envvar:`PYTHON_PYMALLOC_HUGEPAGES` environment variable to ``1``.
  (Contributed by Pablo Galindo Salgado in :gh:`144319`.)
* Annotating anonymous mmap usage is now supported if the Linux kernel supports
  :manpage:`PR_SET_VMA_ANON_NAME <PR_SET_VMA(2const)>` (Linux 5.17 or newer).
  Annotations are visible in ``/proc/<pid>/maps`` if the kernel supports the feature
  and :option:`-X dev <-X>` is passed to Python,
  or Python is built in :ref:`debug mode <debug-build>`.
  (Contributed by Donghee Na in :gh:`141770`.)
* CPython is now built with frame pointers enabled by default
  (:pep:`831`). Pass :option:`--without-frame-pointers` to opt out.
  Authors of C extensions and native libraries built with custom build
  systems should ensure the unwind chain is intact.
  This is usually done by adding ``-fno-omit-frame-pointer`` and
  similar flags to ``CFLAGS``. See :option:`--without-frame-pointers`
  documentation for the specific flags Python uses.
  (Contributed by Pablo Galindo Salgado and Savannah Ostrowski in :gh:`149201`.)
.. _whatsnew315-windows-tail-calling-interpreter:
* 64-bit builds using Visual Studio 2026 (MSVC 18) may now use the new
  :ref:`tail-calling interpreter <whatsnew314-tail-call-interpreter>`.
  Results on Visual Studio 18.1.1 report between
  `15-20% <https://github.com/faster-cpython/ideas/blob/main/results/5800X-msvc.pgo2-vs-msvc.pgo.tc.svg>`__
  speedup on the geometric mean of pyperformance on Windows x86-64 over
  the switch-case interpreter on an AMD Ryzen 7 5800X. We have
  observed speedups ranging from 14% for large pure-Python libraries
  to 40% for long-running small pure-Python scripts on Windows.
  This was made possible by a new feature introduced in MSVC 18,
  which the official Windows 64-bit binaries on python.org__ now use.
  (Contributed by Chris Eibl, Ken Jin, and Brandt Bucher in :gh:`143068`.
  Special thanks to Steve Dower, and the MSVC team including Hulon Jenkins.)
  __ https://www.python.org/downloads/windows/
Porting to Python 3.15
======================
This section lists previously described changes and other bugfixes
that may require changes to your code.
* :class:`sqlite3.Connection` APIs have been cleaned up.
  * All parameters of :func:`sqlite3.connect` except *database* are now keyword-only.
  * The first three parameters of methods :meth:`~sqlite3.Connection.create_function`
    and :meth:`~sqlite3.Connection.create_aggregate` are now positional-only.
  * The first parameter of methods :meth:`~sqlite3.Connection.set_authorizer`,
    :meth:`~sqlite3.Connection.set_progress_handler` and
    :meth:`~sqlite3.Connection.set_trace_callback` is now positional-only.
  (Contributed by Serhiy Storchaka in :gh:`133595`.)
* :data:`resource.RLIM_INFINITY` is now always positive.
  Passing a negative integer value that corresponded to its old value
  (such as ``-1`` or ``-3``, depending on platform) to
  :func:`resource.setrlimit` and :func:`resource.prlimit` is now deprecated.
  (Contributed by Serhiy Storchaka in :gh:`137044`.)
* :meth:`mmap.mmap.resize` has been removed on platforms that don't support the
  underlying syscall, instead of raising a :exc:`SystemError`.
  (Contributed by An Long in :gh:`138205`.)
* A resource warning is now emitted for an unclosed
  :func:`xml.etree.ElementTree.iterparse` iterator if it opened a file.
  Use its :meth:`!close` method or the :func:`contextlib.closing` context
  manager to close it.
  (Contributed by Osama Abdelkader and Serhiy Storchaka in :gh:`140601`.)
* If a short option and a single-dash long option are passed to
  :meth:`argparse.ArgumentParser.add_argument`, *dest* is now inferred from
  the single-dash long option. For example, in ``add_argument('-f', '-foo')``,
  *dest* is now ``'foo'`` instead of ``'f'``.
  Pass an explicit *dest* argument to preserve the old behavior.
  (Contributed by Serhiy Storchaka in :gh:`138697`.)
* Padding of input no longer required in :func:`base64.urlsafe_b64decode`.
  Pass a new argument ``padded=True`` or use :func:`base64.b64decode`
  with argument ``altchars=b'-_'`` (this works with older Python versions)
  to make padding required.
  (Contributed by Serhiy Storchaka in :gh:`73613`.)
* Since :meth:`unittest.TestCase.assertWarns` and
  :meth:`unittest.TestCase.assertWarnsRegex` no longer swallow warnings that
  do not match the specified category or regex, your tests may start leaking
  some warnings that were previously masked.
  Use warning filters to silence them or additional :meth:`!assertWarns*`
  to catch and check them.
  (Contributed by Serhiy Storchaka in :gh:`143231`.)
