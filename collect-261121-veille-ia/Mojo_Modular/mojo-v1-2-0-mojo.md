---
id: collect-261121-veille-ia/mojo-modular/mojo-v1-2-0-mojo
title: "Mojo v1.2.0 | Mojo"
domain: mojo-modular
role: reference
task: article
actors: []
dates: []
keywords: ["mojo", "case", "pattern", "match", "patterns", "value", "statement", "functions"]
source: https://mojolang.org/releases/v1.2.0/
source_anchor: ""
source_lines: [1, 486]
section: "Mojo_Modular"
---
## Highlightsâ

- 
**Pattern matching** : a mechanism for testing whether a value has a particular
structure and optionally extracting pieces of that value into new bindings. A`match` statement compares one value against a series of patterns and runs the
code block for the first pattern that matches. Patterns include literals,
tuples, structs, and many more. You can also use the same patterns in`if` and`while` statements, and in destructuring assignments. See
Language enhancements.
- 
**Math functions on GPU** : more than twenty`std.math` functions no longer
call the system libm. CPU and GPU now run one shared Mojo implementation, so`atan()` ,`tan()` ,`cbrt()` ,`erfc()` ,`lgamma()` ,`log10()` ,`remainder()` ,`hypot()` , the Bessel functions, and others run on accelerators for the
first time. Each function documents its accuracy and subnormal behavior. See
Math functions.
- 
**Faster compilation of large packages** : the`nn` package now compiles in
about 61 seconds, down from about 98 seconds. On Linux the compiler also
uses jemalloc as its allocator, which makes compiling large GPU kernel files
roughly 10 to 13 percent faster by cutting the system time spent in glibc's
arena management. See Tooling changes.
- 
**`StringSpan` is now immutable** :`StringSpan` provides only immutable byte
access. This release removes`MutStringSpan` ,`MutStringSlice` , and the`mut` parameter on`StringSpan` . You need to update code that mutated string data
through a span. See
Strings, text, and hashing.

## Documentationâ

- The Mojo Manual's control flow page and the
compound- and
simple-statement reference pages now
cover `match` ,`comptime match` , patterns,`if case` ,`while case` , and list
destructuring.
- Added a Lexical structure page to the language reference, covering source encoding, lines and indentation, line continuation, comments and docstrings, semicolons, trailing commas, and delimiters.
- Rewrote the Mojo tips for Python devs guide around the Python habits that don't carry over: bracket literals, value semantics, fixed types, argument mutability, numeric representation, structs, traits, and error propagation.
- Added a reference page for the `@inline` decorator, covering its inline levels, explicit level syntax, and use on
parametric functions.

## Language enhancementsâ

- 
Added `match` /`case` pattern matching. The`match` statement compares a
value (the subject) against a set of patterns (the cases):Pattern types include: 
  - Literals (`case 100:` )
  - Or-patterns (`case 100 | 200:` )
  - Tuples (`case (10, 10, 10):` )
  - Guards (`case limit if limit > 10:` )
  - Wildcard (`case _:` )
 And many more. Where a name like `limit` occurs in the pattern, Mojo binds
the name as an immutable reference to that part of the value. Dynamic match
also supports`var` /`ref` bindings.Mojo also supports `comptime match` for testing compile-time values. A`comptime match` statement binds names as parameter values.For an introduction to pattern matching, see the section on the match statement in the Mojo Manual. For comprehensive information on available patterns, see Match statements in the Mojo language reference. This section shows a few illustrative examples of pattern matching: - Literals (
- 
Mojo now supports list destructuring assignments and list patterns for collections that conform to `Sized` and that
are subscriptable. A single`*name` captures the elements between any leading
and trailing patterns:The subject must conform to `Sized` and support integer and`ContiguousSlice` subscripting for patterns that use`*` . A destructuring assignment with the
wrong length raises an error, while a list pattern with the wrong length
doesn't match.
- 
`if case` and`while case` :`if` ,`elif` , and`while` statements now accept
conditional patterns written as`case pattern = value` . Mojo evaluates the
value once, and the names that a successful pattern binds are available in
the statement body. An`if case` pattern failure continues to the next`elif` or`else` arm. A`while case` pattern failure exits the loop normally
and runs its`else` block, if present:- 
You can now write the message on a `where` clause as`where <condition> else "<message>"` , the preferred alternative to the
existing`where (<condition>, "<message>")` spelling, which we plan to
deprecate over time:- 
A struct can now opt out of a trait by writing `not <Trait>` in its
conformance list. This spelling is the preferred alternative to the existing`<Trait> where False` , which continues to work:An opt-out can record the reason, using the same `else "<message>"` spelling as a`where` clause:

## Language changesâ

- 
The `async` and`await` keywords are renamed to`__async` and`__await` to
mark async support as unstable. Write`__async def` and`__await expr` . The
unprefixed spellings still work but emit a warning that async is unstable,
with a fix-it to the prefixed spelling. The prefixed spelling suppresses the
warning but doesn't make async stable.
- 
The empty tuple `()` now always refers to a tuple value and not a type. The
type of an empty tuple is now always written as`Tuple[]` .
- 
Since Mojo 1.1, the walrus operator can only assign to an existing variable, so code such as `if var id := map.get("test"):` fails. Instead, declare the
variable before the statement and assign to it with`:=` :`ref x := ...` has no direct equivalent, since a later`x := ...` assigns
through the reference rather than rebinding it. In a plain`if` , bind it
first with`ref x = ...` , then write`if x:` .

## Library performance improvementsâ

- 
`String` padding and repetition are faster.`ascii_rjust()` ,`ascii_ljust()` ,`ascii_center()` ,
and`*` now build their result in a single allocation, making padding up to
86x faster and repetition about 2x faster on large strings.
- 
`atof()` is faster on long
decimal inputs, cutting a float-heavy parsing corpus from about 2.40 ms to
1.98 ms.

## Library changesâ

### Traits, enums, and reflectionâ

- 
The enum-behaving structs in the standard library now conform to `EnumLike` ,
so they support enum-style pattern matching (`match` with`case .NAME` ) and
reflective case metadata. These include`FastMathFlag` and`TestResult` . Equivalent structs in the
MAX kernels, runtime, and driver APIs gained the same conformance.- 
The default `Writable` implementation now
formats`EnumLike` types by their active case rather than their fields. A case
prints as`TypeName.case(payload)` , or as`TypeName.case` when it has no
payload (`NoneType` ):Types that implement their own `write_to()` or`write_repr_to()` , such as`Optional` and`Bool` , are unaffected.
- 
`Defaultable` now provides a default`__init__()` implementation that default-constructs each field using
reflection. A struct can conform to`Defaultable` without defining an
initializer when all its fields also conform to`Defaultable` .

### Collections, tuples, and spansâ

- 
`Array` now supports slicing with
contiguous slices, returning a`Span` over
the selected elements whose mutability matches the array reference:- 
`Tuple` gained`first_of[T]()` , which returns a
reference to the first element of type`T` . The lookup resolves at compile
time, and it's a compile-time error if the tuple has no element of that type:- 
`Span` has new methods for initializing and deinitializing its elements in
place. The initializing methods take a span of`MaybeUninit[T]` elements and
view the result as a span of`T` :
  - `unsafe_deinit_elements()` destroys every element in place and leaves the memory uninitialized.
  - `unsafe_init_with()` initializes each element with the result of calling a function with that
element's index.
  - `unsafe_init_copy_from()` copies from another span.
  - `unsafe_init_move_from()` moves out of another span.
  - `unsafe_assume_init()` reinterprets the span as initialized, for memory that was initialized some
other way.

### Strings, text, and hashingâ

- 
`StringSpan` now only
provides immutable byte access.`MutStringSpan` ,`MutStringSlice` , and the`mut` parameter on`StringSpan` have been removed.
- 
Hashing a byte sequence is now spelled `hash_bytes()` , which takes an`ImmSpan[Byte]` . The pointer-and-length`hash()` overload is deprecated:`hash()` keeps its`Hashable` meaning, so`hash(my_list)` still hashes a list element by element while`hash_bytes(my_list)` hashes its bytes.
- 
Other hashing changes: 
  - `std.hashlib` was renamed to`std.hash` .
  - `Hasher.finish()` no longer takes
its argument by movable convention.
  - `Hasher` no longer requires an`__init__()` method. You might need to
update some code to require`Defaultable` .
- 
`String.isspace()` and`StringSpan.isspace()` are renamed to`is_ascii_whitespace()` ,
which better describes what they test. The old names still work but are
deprecated.
- 
`String.startswith()` and`String.endswith()` now cost time proportional to the affix rather than to the whole string, which
makes them orders of magnitude faster on large strings. Their edge cases now
match Python: an empty affix is true at any in-range offset, a negative`start` or`end` counts back from the end, and an`end` past the end of the
string clamps.

### SIMD and byte orderâ

- 
`SIMD.from_bytes()` and`SIMD.as_bytes()` now take an`endian` parameter of the new`Endian` type (`Endian.big` or`Endian.little` ) instead of the`big_endian``Bool` :The default is still the target's byte order, which `Endian.native()` returns.`is_big_endian()` and`is_little_endian()` are deprecated;
compare`Endian.native()` with`.big` or`.little` instead.
- 
`SIMD` gained`to_endian()` and`from_endian()` , which convert an integer
value to and from a given byte order:

### Math functionsâ

- 
Many `std.math` functions are now implemented in Mojo
rather than calling the system libm, so they run on GPU as well as CPU, and
CPU and GPU share one implementation. Except where noted, CPU results can
differ from the system libm in the last ulp:
  - `acosh()` and`asinh()` .
  - `atan()` and`atan2()` . On targets that flush subnormals,
subnormal inputs are handled and normal results are correct; subnormal
results flush to zero.
  - `atanh()` for`float64` , so its`float64` form
now runs on GPUs that have`float64` . All floating-point types share one
implementation, so`float32` GPU results, previously up to 16 ulp off
through a target-dependent`log` , are now within 1 ulp of the CPU. Results
can also differ between targets in the last ulp. On targets that flush
subnormals, subnormal inputs are handled and normal results are correct;
subnormal results flush to zero.
  - The Bessel functions `j0()` ,`j1()` ,`y0()` , and`y1()` . For`float32` , relative error grows near
the zeros of each function, while absolute error stays small there. On
targets that flush subnormals, subnormal inputs and subnormal results are
handled; only`j1()` can produce a subnormal result in`float32` , and it's
kept.
  - `cbrt()` , on GPU for every floating-point type,
including`float64` .`float32` results are within 0.505 ulp of the true
root.
  - `erfc()` for`float64` .`float64``erf()` now uses a`float64` approximation and
is accurate to about 1 ulp. Both share one implementation with`float32``erfc()` .
  - `expm1()` for`float64` . On targets that flush
subnormals, subnormal inputs are handled; an input whose result would be
subnormal is returned bit-exactly, so no subnormal result is ever flushed.
  - `gamma()` . Subnormal inputs and results are
also handled on targets that flush subnormals, such as Apple GPUs in`float32` .
  - `hypot()` .
  - `lgamma()` . It stays within about 2 ulp for
negative inputs, including next to the negative real zeros of`lgamma` (such
as -2.457 and -2.748), where the result is near 0.
  - `log10()` , which no longer calls GPU
intrinsics either: CPU and every GPU now run the same implementation,
accurate to within 1 ulp on measured targets, and`float64``log10()` now
runs on AMD GPUs. Results can also differ between targets in the last ulp
where the compiler contracts a multiply-add into an fma. On targets that
flush subnormals, subnormal inputs are handled and normal results are
correct; subnormal results flush to zero.
  - `logb()` , which is exact: it reads the exponent
from the bit pattern, so subnormal inputs return their true exponent even on
GPUs that flush subnormals.
  - `nextafter()` , in`std.utils.numerics` , which is exact and
matches the system libm for non-NaN inputs because it steps the IEEE-754 bit
pattern directly. Since every ordering, zero, and equality predicate reads
the raw bits, it's unaffected by targets that flush subnormal arithmetic.`std.math.ulp()` , which is defined through`nextafter()` , follows it onto the GPU.
  - `remainder()` , which is exact, so it
matches the system libm bit for bit. On targets that flush subnormals,
subnormal inputs and subnormal results are handled; the results match the
system libm bit for bit.
  - `scalb()` , whose results are bit-exact against
IEEE targets. Apple GPUs don't support`float64` . On targets that flush
subnormals, subnormal inputs and subnormal results are handled.
  - `tan()` . CPU and GPU results can also differ
from each other in the last ulp. On targets that flush subnormals, subnormal
inputs are handled and normal results are correct; subnormal results flush
to zero.
- 
`float64``log()` now uses double-precision
coefficients and is within 1 ulp of the correctly rounded result; it was
previously off by up to about 2e7 ulp. It runs the same Mojo code on CPU and
GPU.
- 
`log1p()` now evaluates in the input's own
precision with a Mojo implementation based on musl, shared by CPU and GPU, and
is accurate to within 1 ulp for`float32` and`float64` .`float16` and`bfloat16` inputs are computed in`float32` .`float64``log1p()` is no longer
supported on Apple GPUs, which have no`float64` arithmetic.
- 
`std.math` gained`mulwide()` and`mulhi()` , which compute the full-width product
and the high half of a 32- or 64-bit integer multiply. They are no longer
GPU-only.

### Compilation targets and acceleratorsâ

- 
The public `CompilationTarget` type
(introduced in Mojo 1.1) is now used in most APIs that are parameterized on
compilation target, such as`size_of()` ,`align_of()` , and`compile_info()` .Additionally, `CompilationTarget` now provides predicate methods for checking
the vendor identity of a given target. The preexisting free functions with the
same name now call these new methods. These include:
  - `.is_nvidia_gpu()` for checking whether a compilation target is an NVIDIA GPU.
  - `.is_amd_gpu()` and`.is_apple_gpu()` provide the equivalent checks for AMD and Apple GPUs, respectively.
- 
`std.sys` gained a`current_accelerator()` free
function, which returns the accelerator target the current code is being
compiled for and fails at compile time when that target isn't an accelerator.
From host code, use`default_accelerator()` ,
which returns the default accelerator target, either determined automatically
or as specified by`--target-accelerator` . Both functions return a`CompilationTarget` . In nightly builds after this release,`default_accelerator()` moves to`max.gpu.host.info` .
- 
Added a new `TargetAccelerator` type for representing accelerator metadata at compile time, combining`GPUInfo` and`CompilationTarget` values.Target constants such as `B200` and`MI355X` are now`TargetAccelerator` instances rather than`GPUInfo` instances.Use `.gpu_info` to access the prior device metadata, or`.target` for the
compilation target.

### Unified closuresâ

- 
`Tuple.consume_elements()` ,`Tuple.deinit_with()` , and`VariadicPack.consume_elements()` now take the element handler as a runtime closure argument instead of a
compile-time`capturing` parameter, matching`List.deinit_with()` and`VariadicList.consume_elements()` :- 
`SIMD.reduce()` now takes a closure reduction
function as a runtime argument instead of a compile-time`capturing` parameter. The`thin` function-pointer form is unchanged:

### Other library changesâ

- 
`FileDescriptor` is now`RawFileDescriptor` , and it
takes its descriptor as a`c_int` , matching the C`int` type that the underlying descriptor APIs take. Integer literals keep
constructing one unchanged, but passing an`Int` variable now requires an
explicit`c_int()` conversion. The underlying descriptor value is no longer a
public field. The descriptor value`-1` is the type's niche, so`Optional[RawFileDescriptor]` is the same size as`RawFileDescriptor` , and
constructing a`RawFileDescriptor` from a negative value aborts; use`Optional[RawFileDescriptor]` to represent an absent descriptor.
- 
`Layout` now carries its alignment as a
keyword-only parameter of the new`Alignment` type, instead of storing it
as a runtime field. It defaults to the element type's natural alignment, so`Layout[T](count=n)` is unchanged. Build an`Alignment` with`Alignment.of[T]()` for a type's natural alignment or`Alignment.of_bytes[n]()` for an explicit byte count.`Allocation` and`ManagedAllocation` carry the
same parameter.`ThinAllocation` deliberately doesn't: it records only the element type, so the alignment
travels with the`Layout` you supply to`unsafe_with_layout()` .For now, Mojo supports only compile-time alignments. This keeps the common case (natural type alignment) simple, so the `Layout` and`Allocation` types
don't pay the cost of holding an extra`Int` field. Dynamic (runtime)
alignment will come after more design work.
- 
`Coord.product()` has a new
parameterized overload,`product[T: DType]()` , which accumulates and returns
the product at`T` rather than at`Coord.DTYPE` :The unparameterized `product()` is unchanged and still returns`Scalar[Coord.DTYPE]` . A`T` too narrow to hold the result wraps rather than
widening, so a caller that picks one owns the overflow.
- 
`Coord` has a new`replace_dynamic[at](value)` method, the in-place counterpart to`replace()` . It keeps the coordinate's
type, so the element at`at` must already be dynamic, and`value` takes that
element's own dtype:- 
`CoordLike` now derives from`Intable` and supplies a default`__int__()` , so any coordinate-like value, including`ComptimeInt` , converts with`Int(c)` .
- 
The uninitialized-read check now accepts `MOJO_STDLIB_SIMD_UNINIT_CHECK=report` ,
which prints the same diagnostic but keeps running, so a single run enumerates
every offending site instead of aborting at the first one. Apple GPUs, which
cannot print the diagnostic, still trap. The default`abort` behavior is
unchanged.

## Tooling changesâ

- The `mojo` compiler now uses jemalloc as its allocator on Linux. Compiling
large GPU kernel files is roughly 10 to 13 percent faster because the
compiler spends far less system time in glibc's arena management.
- The Mojo language server now reports a value's address space as a semantic
token modifier (`addressSpace0` through`addressSpace15` , matching the`AddressSpace` index) so
editors can color shared, local, and constant memory differently. The address
space is found in the value's type or in a type nested in its parameters, so
an`Array` of shared pointers is marked too.
- Compiling large Mojo packages is significantly faster. The `nn` package now
compiles in about 61 seconds, down from about 98 seconds.
- `mojo build --emit` can now write one output file per offload kernel,
alongside whatever artifact the kernel already embeds.`--emit` validates the
requested kind against the selected target and renders its help from the
target's own declared kinds. Metal targets gained an`air` kind, which emits
one optimized Apple AIR bitcode file per kernel.

## Removedâ

- 
Removed `has_nvidia_gpu_accelerator()` ,`has_amd_gpu_accelerator()` , and`has_apple_gpu_accelerator()` from`std.sys` , including the sub-architecture
overloads of the NVIDIA variant. Ask the target itself instead, using the
new`CompilationTarget` predicate methods:Code that already holds a device context can check its target directly, for example `ctx.target.is_nvidia_gpu()` .
- 
Removed `sum()` from the`CoordLike` trait and from its implementations
(`Coord` ,`ComptimeInt` , and the`All` marker). Nothing called it: a
coordinate's elements are extents and indices, so adding them together
carries no meaning, unlike`product()` , whose result is the number
of elements a shape describes. Use`product()` for that, or iterate the
elements and add them yourself if you really want a sum.
- 
Removed `Coord.inner_product()` .`Coord` describes extents and indices, so
treating two coordinates as Euclidean vectors had no meaning; use`product()` ,
or compute the sum of products yourself.

## Fixedâ

### Language and type checkingâ

- 
Struct layouts that recurse only through a parametric indirection now compile. A field such as `Optional[List[Pointer[Node, origin]]]` or`Tuple[Pointer[Node, origin], Int]` inside`struct Node` was spuriously
rejected with`struct has recursive reference to itself` , despite the
recursion being behind a pointer.
- 
Passing `x^` to an`imm` or`ref` argument now warns that the move has
no effect, instead of silently ignoring it.
- 
#7153 - t-string interpolations are now captured by reference, so mutating an interpolated `Int` before the t-string renders no longer prints a stale value.
- 
#7204 - Contextual member expressions now accept runtime arguments. A call such as `paint(.gray(level))` , where`level` is a runtime value, now resolves against
the parameter type.
- 
An extension of a struct that hasn't been resolved yet no longer fails to check its members or reports a spurious cycle.
- 
Struct extensions reached through a wildcard import of a precompiled package are no longer silently dropped.
- 
A method reached through an extension that is visible from more than one enclosing scope no longer reports a spurious `ambiguous call` error.
- 
A function whose argument is typed with a constrained parametric comptime alias can now use that constraint in its body.
- 
Slice-literal subscripts now work on types that define both `__getitem_param__()` and`__getitem__()` . A slice literal with no contextual
type falls back to`ContiguousSlice` , or to`Slice` when it has a stride.
- 
A `try` block that infers its error type now rejects an error naming a field
of a local declared in the try body, instead of failing later with an IR
verification error.
- 
`origin_of()` and origin specifiers now report an error when given a
temporary,
such as`ref [String("lit")] z: Int` , instead of failing later with an
internal compiler error.
- 
#6718 - A trailing `finally` that follows a nested`try` /`except` now parses correctly instead
of being absorbed into the inner block.

### Compiler crashes and hangsâ

- 
#7164 - Fixed a compiler crash at `-O2` and`-O3` on code whose values flowed only into pure
operations with an unused final result.
- 
#7127 - Fixed the compiler hanging in lifetime checking on a nested `for` loop that assigns to a
variable declared in the outer loop body.
- 
Move-capturing a trivially register-passable value in a closure capture list no longer crashes the compiler.
- 
Calling the initializer of a parametric struct that raises a convertible error type no longer crashes the compiler.
- 
Reflecting on a function whose mangled name contains escaped quotes no longer crashes the compiler.
- 
Compile-time code that builds a cyclic data structure, such as a doubly linked list, no longer overflows the compiler's stack.
- 
Misusing a kernel entry function now reports a compiler error instead of an internal assertion failure.

### Code generation and optimizationâ

- 
Atomic updates to a value passed in memory are no longer dropped by the optimizer.
- 
#7155 - A value used on a path that leads to a no-return call, such as `abort()` , is no longer destroyed before the
branches rejoin.
- 
#7154 - Integer constants of different widths no longer collide during constant folding, a bug that made `Int(UInt32.MAX)` fold to`-1` instead of`4294967295` .

### Math and numericsâ

- 
`atof()` now rounds large mantissas correctly and accepts numbers with more
than 19 significant digits:
  - Values whose significant digits exceed 53 bits, such as
`atof("123456789012345678")` , could round to the wrong neighboring
double, and exact ties such as`atof("4503599627370497.5")` did not round
to even.
  - Inputs with more than 19 significant digits previously raised `The number is too long` . Malformed inputs such as`"1..2"` ,`"1e5.0"` , or`"1e--5"` ,
which the old parser silently misread by skipping the offending
characters, now raise.
- Values whose significant digits exceed 53 bits, such as
- 
On macOS, `scalb()` now returns NaN for a non-integral exponent, as it does
on Linux.
- 
`float32``expm1()` no longer overflows early for inputs just below 88.7,
and it now returns`-0.0` for`-0.0` and`inf` for`inf` .
- 
`float32``erfc()` is now accurate in its tail, for large positive inputs,
and returns subnormal results instead of flushing them to zero. On targets
that flush subnormals, subnormal inputs are handled and normal results are
correct; subnormal results flush to zero.
- 
`cbrt()` of`inf` and`-inf` now returns`inf` and`-inf` instead of`Float32.MAX` and`-Float32.MAX` , and`float32``cbrt()` is correct near`Float32.MAX` and for subnormal inputs. On targets that flush subnormals,
subnormal inputs are handled and normal results are correct; subnormal
results flush to zero, though`cbrt()` never produces one.
- 
#7302 - `log()` ,`log2()` , and`log1p()` of`inf` now return`inf` instead of`nan` .
- 
#7101 - `float64``log1p()` is now accurate for inputs whose`1 + x` falls in`[sqrt(2)/2, sqrt(2))` ,
where it was off by up to about 7e9 ulp.
- 
`sqrt()` ,`rsqrt()` , and`exp2()` now compile when evaluated at compile
time inside a Metal kernel.`float16``sin()` and`cos()` now use the native half-precision Metal
intrinsics.
- 
`std.math.frexp()` now returns the correct
fraction and exponent for subnormal inputs, and handles values at the top of
the exponent range.

### SIMD and GPUâ

- 
`SIMD.insert()` now compiles when the inserted
value is a`Scalar` , and on Apple GPUs.
- 
`Optional` and`OptionalReg` now convert a`DevicePassable` payload
to its device representation before it reaches a GPU kernel.
- 
A closure that copy-captures a type with a conditional `RegisterPassable` conformance,
such as`Tuple[Int32, Int32]` , is now`RegisterPassable` when the capture
satisfies the condition, and`DevicePassable` when every capture is also`DevicePassable` .

### Other fixesâ

- 
#7261 - `c_char` now reflects the target's actual`char` signedness: unsigned by default on Linux running on ARM (AAPCS64), signed
everywhere else Mojo currently targets (x86-64 Linux, macOS on Apple
silicon). Previously`c_char` was always signed, which could bind an FFI
declaration to the wrong C type on Linux/ARM.
- 
`String.split()` and`StringSpan.split()` no longer put the trailing empty slice out of bounds when splitting on an
empty separator.
- 
Mojo tools no longer send the headers in `OTEL_EXPORTER_OTLP_HEADERS` , or
its signal-specific forms, to Modular's telemetry collector.

## Special thanksâ

Special thanks to our community contributors:

Angel Marinov (@0xamlab), Arseni Ivanov (@arseniivanov), David Dada (@obadafidii), Giorgos Smyridis (@gsmyridis), Jay Hemnani (@jayhemnani9910), KrisztiÃ¡n SzÅ±cs (@kszucs), Londo (@Londopy), Mahendra Rathore (@mahendrarathore1742), Manas (@weirdsmiley), Marius S (@winding-lines), Narayan (@vortex73), Oleksandr Tarasiuk (@a-tarasyuk), Wei-Cheng Hsu (@weicheng-hsu)
