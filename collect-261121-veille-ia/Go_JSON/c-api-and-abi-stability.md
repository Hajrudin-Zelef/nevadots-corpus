---
id: collect-261121-veille-ia/go-json/c-api-and-abi-stability
title: "C API and ABI Stability"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["python", "stable", "only", "compatible", "extensions", "abi3t", "abi3", "compiled"]
source: https://docs.python.org/3.15/c-api/stable.html
source_anchor: ""
source_lines: [1, 314]
section: "Go_JSON"
---
# C API and ABI StabilityÂ¶

Unless documented otherwise, Pythonâs C API is covered by the Backwards
Compatibility Policy, **PEP 387**.
Most changes to it are source-compatible (typically by only adding new API).
Changing existing API or removing API is only done after a deprecation period
or to fix serious issues.

CPythonâs Application Binary Interface (ABI) is forward- and backwards-compatible across a minor release (if these are compiled the same way; see Platform Considerations below). So, code compiled for Python 3.10.0 will work on 3.10.8 and vice versa, but will need to be compiled separately for 3.9.x and 3.11.x.

There are two tiers of C API with different stability expectations:

- Unstable API, may change in minor versions without a deprecation period. It is marked by the `PyUnstable` prefix in names.
- Limited API, is compatible across several minor releases. When `Py_LIMITED_API` is defined, only this subset is exposed
from`Python.h` .

These are discussed in more detail below.

Names prefixed by an underscore, such as `_Py_InternalState`,
are private API that can change without notice even in patch releases.
If you need to use this API, consider reaching out to
CPython developers
to discuss adding public API for your use case.

## Unstable C APIÂ¶

Any API named with the `PyUnstable` prefix exposes CPython implementation
details, and may change in every minor release (e.g. from 3.9 to 3.10) without
any deprecation warnings.
However, it will not change in a bugfix release (e.g. from 3.10.0 to 3.10.1).

It is generally intended for specialized, low-level tools like debuggers.

Projects that use this API are expected to follow CPython development and spend extra effort adjusting to changes.

## Stable Application Binary InterfacesÂ¶

Pythonâs *Stable ABI* allows extensions to be compatible with multiple
versions of Python, without recompilation.

Note

For simplicity, this document talks about *extensions*, but Stable ABI
works the same way for all uses of the API â for example, embedding Python.

There are two Stable ABIs:

- `abi3` , introduced in Python 3.2, is compatible with**non** -free-threaded builds of CPython.
- `abi3t` , introduced in Python 3.15, is compatible with
free-threaded builds of CPython.
It has stricter API limitations than`abi3` .

It is possible for an extension to be compiled for *both* `abi3` and
`abi3t` at the same time; the result will be compatible with
both free-threaded and non-free-threaded builds of Python.
Currently, this has no downsides compared to compiling for `abi3t` only.

Each Stable ABI is versioned using the first two numbers of the Python version. For example, Stable ABI 3.14 corresponds to Python 3.14. An extension compiled for Stable ABI 3.x is ABI-compatible with Python 3.x and above.

Extensions that target a stable ABI must only use a limited subset of
the C API. This subset is known as the *Limited API*; its contents
are listed below.

On Windows, extensions that use a Stable ABI should be linked against
`python3.dll` rather than a version-specific library such as
`python39.dll`.
This library only exposes the relevant symbols.

On some platforms, Python will look for and load shared library files named
with the `abi3` or `abi3t` tag (for example, `mymodule.abi3.so`).
Free-threaded interpreters only recognize the
`abi3t` tag, while non-free-threaded ones will prefer `abi3` but fall back
to `abi3t`.
Thus, extensions compatible with both ABIs should use the `abi3t` tag.

Python does not necessarily check that extensions it loads
have compatible ABI.
Extension authors are encouraged to add a check using the `Py_mod_abi`
slot or the `PyABIInfo_Check()` function, but the user
(or their packaging tool) is ultimately responsible for ensuring that,
for example, extensions built for Stable ABI 3.10 are not installed for lower
versions of Python.

All functions in Stable ABI are present as functions in Pythonâs shared
library, not solely as macros.
This makes them usable in languages that donât use the C
preprocessor, including Pythonâs `ctypes`.

### Compiling for Stable ABIÂ¶

Note

Build tools (such as, for example, meson-python, scikit-build-core, or Setuptools) often have a mechanism for setting macros and synchronizing them with extension filenames and other metadata. Prefer using such a mechanism, if it exists, over defining the macros manually.

The rest of this section is mainly relevant for tool authors, and for people who compile extensions manually.

To compile for a Stable ABI, define one or both of the following macros
to the lowest Python version your extension should support, in
`Py_PACK_VERSION` format.
Typically, you should choose a specific value rather than the version of
the Python headers you are compiling against.

The macros must be defined before including `Python.h`.
Since `Py_PACK_VERSION` is not available at this point, you
will need to use the numeric value directly.
For reference, the values for a few recent Python versions are:

```
0x30a0000  /* Py_PACK_VERSION(3,10) */
0x30b0000  /* Py_PACK_VERSION(3,11) */
0x30c0000  /* Py_PACK_VERSION(3,12) */
0x30d0000  /* Py_PACK_VERSION(3,13) */
0x30e0000  /* Py_PACK_VERSION(3,14) */
0x30f0000  /* Py_PACK_VERSION(3,15) */
```
When one of the macros is defined, `Python.h` will only expose API that is
compatible with the given Stable ABI â that is, the
Limited API plus some definitions that need to be
visible to the compiler but should not be used directly.
When both are defined, `Python.h` will only expose API compatible with
both Stable ABIs.

- 
Py_LIMITED_APIÂ¶
- Target `abi3` , that is,
non-free-threaded builds of CPython.
See above for common information.

- 
Py_TARGET_ABI3TÂ¶
- Target `abi3t` , that is,
free-threaded builds of CPython.
See above for common information.Added in version 3.15.

Both macros specify a target ABI; the different naming style is due to backwards compatibility.

Historical note

You can also define `Py_LIMITED_API` as `3`. This works the same as
`0x03020000` (Python 3.2, the version that introduced Stable ABI).

When both are defined, `Python.h` may, or may not, redefine
`Py_LIMITED_API` to match `Py_TARGET_ABI3T`.

On a free-threaded build â that is, when
`Py_GIL_DISABLED` is defined â `Py_TARGET_ABI3T`
defaults to the value of `Py_LIMITED_API`.
This means that there are two ways to build for both `abi3` and `abi3t`:

- define both `Py_LIMITED_API` and`Py_TARGET_ABI3T` , or
- define only `Py_LIMITED_API` and:
  - on Windows, define `Py_GIL_DISABLED` ;
  - on other systems, use the headers of free-threaded build of Python.

### Stable ABI Scope and PerformanceÂ¶

The goal for Stable ABI is to allow everything that is possible with the full C API, but possibly with a performance penalty. Generally, compatibility with Stable ABI will require some changes to an extensionâs source code.

For example, while `PyList_GetItem()` is available, its âunsafeâ macro
variant `PyList_GET_ITEM()` is not.
The macro can be faster because it can rely on version-specific implementation
details of the list object.

For another example, when *not* compiling for Stable ABI, some C API
functions are inlined or replaced by macros.
Compiling for Stable ABI disables this inlining, allowing stability as
Pythonâs data structures are improved, but possibly reducing performance.

By leaving out the `Py_LIMITED_API` or `Py_TARGET_ABI3T`
definition, it is possible to compile Stable-ABI-compatible source
for a version-specific ABI.
A potentially faster version-specific extension can then be distributed
alongside a version compiled for Stable ABI â a slower but more compatible
fallback.

### Stable ABI CaveatsÂ¶

Note that compiling for Stable ABI is *not* a complete guarantee that code will
be compatible with the expected Python versions.
Stable ABI prevents *ABI* issues, like linker errors due to missing
symbols or data corruption due to changes in structure layouts or function
signatures.
However, other changes in Python can change the *behavior* of extensions.

One issue that the `Py_TARGET_ABI3T` and `Py_LIMITED_API`
macros do not guard against is calling a function with arguments that are
invalid in a lower Python version.
For example, consider a function that starts accepting `NULL` for an
argument. In Python 3.9, `NULL` now selects a default behavior, but in
Python 3.8, the argument will be used directly, causing a `NULL` dereference
and crash. A similar argument works for fields of structs.

For these reasons, we recommend testing an extension with *all* minor Python
versions it supports.

We also recommend reviewing documentation of all used API to check
if it is explicitly part of the Limited API. Even with `Py_LIMITED_API`
defined, a few private declarations are exposed for technical reasons (or
even unintentionally, as bugs).

Also note that while compiling with `Py_LIMITED_API` 3.8 means that the
extension should *load* on Python 3.12, and *compile* with Python 3.12,
the same source will not necessarily compile with `Py_LIMITED_API`
set to 3.12.
In general, parts of the Limited API may be deprecated and removed,
provided that Stable ABI stays stable.

## Platform ConsiderationsÂ¶

ABI stability depends not only on Python, but also on the compiler used, lower-level libraries and compiler options. For the purposes of the Stable ABIs, these details define a âplatformâ. They usually depend on the OS type and processor architecture

It is the responsibility of each particular distributor of Python
to ensure that all Python versions on a particular platform are built
in a way that does not break the Stable ABIs, or the version-specific ABIs.
This is the case with Windows and macOS releases from `python.org` and many
third-party distributors.

## ABI CheckingÂ¶

Added in version 3.15.

Python includes a rudimentary check for ABI compatibility.

This check is not comprehensive. It only guards against common cases of incompatible modules being installed for the wrong interpreter. It also does not take platform incompatibilities into account. It can only be done after an extension is successfully loaded.

Despite these limitations, it is recommended that extension modules use this mechanism, so that detectable incompatibilities raise exceptions rather than crash.

Most modules can use this check via the `Py_mod_abi`
slot and the `PyABIInfo_VAR` macro, for example like this:

```
PyABIInfo_VAR(abi_info);
static PyModuleDef_Slot mymodule_slots[] = {
   {Py_mod_abi, &abi_info},
   ...
};
```
The full API is described below for advanced use cases.

- 
int PyABIInfo_Check(PyABIInfo *info, const char *module_name)Â¶
-  *Part of the Stable ABI since version 3.15.*Verify that the given *info* is compatible with the currently running
interpreter.Return 0 on success. On failure, raise an exception and return -1. If the ABI is incompatible, the raised exception will be `ImportError` .The *module_name* argument can be`NULL` , or point to a NUL-terminated
UTF-8-encoded string used for error messages.Note that if *info* describes the ABI that the current code uses (as defined
by`PyABIInfo_VAR` , for example), using any other Python C API
may lead to crashes.
In particular, it is not safe to examine the raised exception.Added in version 3.15.

- 
PyABIInfo_VAR(NAME)Â¶
-  *This macro is available when compiling for the Stable ABI since version 3.15.*Define a static `PyABIInfo` variable with the given*NAME* that
describes the ABI that the current code will use.
This macro expands to:static PyABIInfo NAME = { 1, 0, PyABIInfo_DEFAULT_FLAGS, PY_VERSION_HEX, PyABIInfo_DEFAULT_ABI_VERSION } Added in version 3.15.

- 
type PyABIInfoÂ¶
-  *Part of the Stable ABI (including all members) since version 3.15.*
  - 
uint8_t abiinfo_major_versionÂ¶
  - The major version of `PyABIInfo` . Can be set to:
    - `0` to skip all checking, or
    - `1` to specify this version of`PyABIInfo` .
 
  - 
uint8_t abiinfo_minor_versionÂ¶
  - The minor version of `PyABIInfo` .
Must be set to`0` ; larger values are reserved for backwards-compatible
future versions of`PyABIInfo` .
 
  - 
uint16_t flagsÂ¶
  - This field is usually set to the following macro: 
    - 
PyABIInfo_DEFAULT_FLAGSÂ¶
    -  *Part of the Stable ABI since version 3.15.*Default flags, based on current values of macros such as `Py_LIMITED_API` and`Py_GIL_DISABLED` .
 Alternately, the field can be set to the following flags, combined by bitwise OR. Unused bits must be set to zero. ABI variant â one of: Free-threading compatibility â one of: 
    - 
PyABIInfo_FREETHREADEDÂ¶
    -  *Part of the Stable ABI since version 3.15.*Specifies ABI compatible with free-threaded builds of CPython. (That is, ones compiled with `--disable-gil` ; with`t` in`sys.abiflags` )
 
    - 
PyABIInfo_GILÂ¶
    -  *Part of the Stable ABI since version 3.15.*Specifies ABI compatible with non-free-threaded builds of CPython (ones compiled *without*`--disable-gil` ).
 
    - 
PyABIInfo_FREETHREADING_AGNOSTICÂ¶
    -  *Part of the Stable ABI since version 3.15.*Specifies ABI compatible with both free-threaded and non-free-threaded builds of CPython, that is, both `abi3` and`abi3t` .
  - 
PyABIInfo_DEFAULT_FLAGSÂ¶
 
  - 
uint32_t build_versionÂ¶
  - The version of the Python headers used to build the code, in the format used by `PY_VERSION_HEX` .This can be set to `0` to skip any checks related to this field.
This option is meant mainly for projects that do not use the CPython
headers directly, and do not emulate a specific version of them.
 
  - 
uint32_t abi_versionÂ¶
  - The ABI version. For Stable ABI, this field should be the value of `Py_LIMITED_API` or`Py_TARGET_ABI3T` .
If both are defined, use the smaller value.
(If`Py_LIMITED_API` is`3` ; use
Py_PACK_VERSION(3, 2) instead of`3` .)Otherwise, it should be set to `PY_VERSION_HEX` .It can also be set to `0` to skip any checks related to this field.
    - 
PyABIInfo_DEFAULT_ABI_VERSIONÂ¶
    -  *Part of the Stable ABI since version 3.15.*The value that should be used for this field, based on current values of macros such as `Py_LIMITED_API` ,`PY_VERSION_HEX` and`Py_GIL_DISABLED` .
  - 
PyABIInfo_DEFAULT_ABI_VERSIONÂ¶
 Added in version 3.15.
- 
uint8_t abiinfo_major_versionÂ¶

## Contents of Limited APIÂ¶

This is the definitive list of Limited API for Python 3.15:
