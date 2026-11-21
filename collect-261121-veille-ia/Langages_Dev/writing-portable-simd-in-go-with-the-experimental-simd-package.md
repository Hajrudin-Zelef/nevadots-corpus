---
id: collect-261121-veille-ia/langages-dev/writing-portable-simd-in-go-with-the-experimental-simd-package
title: "Writing Portable SIMD in Go with the Experimental simd Package"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simd", "package", "lanes", "vector", "mask", "portable", "experimental", "writing"]
source: https://www.bytesizego.com/blog/writing-portable-simd-in-go-with-the-experimental-simd-packa
source_anchor: ""
source_lines: [1, 137]
section: "Langages_Dev"
---
# Writing Portable SIMD in Go with the Experimental simd Package

Go looking for `Float32x4` in the new `simd` package and you wonât find it. No `Int8x32` either. Vector width is absent from the type system on purpose: you write `simd.Float32s`, ask it how many lanes it has at run time with `Len()`, and the compiler emits specialized copies of your function for 128-bit, 256-bit, and 512-bit hardware, plus a pure-software fallback. One source file, every architecture.

Thatâs the experimental platform-independent SIMD API described in the official Go blog post. Go 1.26 shipped the architecture-specific `archsimd` package for amd64; Go 1.27 added arm64 NEON and wasm, then layered the portable `simd` package on top. The design borrows heavily from Highway for C++.

Everything below needs `GOEXPERIMENT=simd` and a `//go:build goexperiment.simd` constraint on your files.

## Vector length is a runtime value, not a type parameter

If youâve written generic Go, the instinct is to reach for type parameters to abstract over vector width. The `simd` package refuses. The types are capitalized plural primitives, `simd.Uint8s`, `simd.Float32s`, `simd.Int64s`, and you load them from and store them to slices.

The payoff is that the loop shape never changes: read the lane count, stride by it, handle the tail.

```
//go:build goexperiment.simd
package vec
import "simd"
// Clamp limits every element of xs to [lo, hi], in place.
func Clamp(xs []float32, lo, hi float32) {
	loV := simd.BroadcastFloat32s(lo)
	hiV := simd.BroadcastFloat32s(hi)
	// Lane count depends on the hardware this binary is running on:
	// 4 for NEON/wasm, up to 16 for AVX512.
	w := loV.Len()
	i := 0
	for ; i+w <= len(xs); i += w {
		v := simd.LoadFloat32s(xs[i : i+w])
		v = v.Max(loV).Min(hiV)
		v.Store(xs[i : i+w])
	}
	// Tail: fewer than w elements remain.
	if i < len(xs) {
		v, _ := simd.LoadFloat32sPart(xs[i:])
		v = v.Max(loV).Min(hiV)
		v.StorePart(xs[i:])
	}
}
```
`LoadFloat32sPart` returns the vector plus the number of elements it actually read, and fills the remaining lanes with zero. Here, `StorePart` writes back only `len(xs[i:])` elements, so the clampâs changes to the unused lanes never reach memory. This load-part/store-part pair replaces the scalar epilogue loop youâd otherwise write by hand, which is where a surprising share of SIMD bugs live.

Worth watching, though: the partial lanes still participate in the arithmetic. Harmless for `Min` and `Max`. Not harmless if youâre accumulating. When you fold the tail into a running sum you want zero-filled lanes, and zero-filled lanes are exactly what `LoadPart` hands you. Thatâs why the inner-product example in the Go blog can reuse `MulAdd` for the tail with no special casing.

## Masks are per-width types

Comparisons donât return a vector of `bool`. They return a mask whose type is welded to the element width: comparing `Int8s` gives you `Mask8s`, comparing `Float32s` or `Int32s` gives you `Mask32s`. The compiler will not let you apply a 32-bit mask to a vector of 8-bit lanes. Thatâs the single best thing in the API, because itâs precisely the bug class that eats afternoons when you write this in assembly.

Two main ways to apply a mask. `x.Masked(m)` keeps `x` where the mask bit is set and zeroes the rest. `x.IfElse(m, y)` picks between two vectors lane by lane.

```
//go:build goexperiment.simd
package vec
import "simd"
// ZeroBelow sets every element smaller than limit to zero, in place.
func ZeroBelow(xs []float32, limit float32) {
	limV := simd.BroadcastFloat32s(limit)
	w := limV.Len()
	i := 0
	for ; i+w <= len(xs); i += w {
		v := simd.LoadFloat32s(xs[i : i+w])
		keep := v.GreaterEqual(limV) // Mask32s
		v.Masked(keep).Store(xs[i : i+w])
	}
	if i < len(xs) {
		v, _ := simd.LoadFloat32sPart(xs[i:])
		keep := v.GreaterEqual(limV)
		v.Masked(keep).StorePart(xs[i:])
	}
}
```
Underneath, the platforms barely agree on what a mask is. AVX512 and RVV have dedicated mask registers with one bit per element. SVE uses one bit per vector *byte*. AVX, AVX2, NEON, and wasm have no mask registers at all and fake it with vector bitmasks and boolean ops. `Mask32s` papers over all of that, and the abstraction holds because the operations exposed on masks are the ones every representation can do cheaply.

## What is missing, and why

The Go 1.27 API is the intersection of what every supported architecture can do, with gaps filled by emulation. That intersection is smaller than youâd guess. From the official documentation:

- Thereâs no horizontal sum in 1.27. To add up the lanes of a vector you `Store` into a slice and loop.`ReduceSum` is planned for the next release.
- `Div` and`Sqrt` exist only for the float types.`MulAdd` too.
- Hardware gaps do not always become API gaps. wasm has no native comparisons for 64-bit integer vectors, but the portable package can emulate operations when the cost is reasonable.
- `Average` is limited to`Uint8s` and`Uint16s` ;`AddSaturated` and`SubSaturated` are limited to the signed and unsigned 8- and 16-bit types.

Some gaps get emulated instead of dropped. Scalar shift distances turn into vector shifts where the hardware only offers the latter. A missing unsigned comparison becomes a signed comparison plus two XORs with a constant. Carryless multiply, which CRC and crypto need, is emulated where absent, and the emulation is written so its run time doesnât depend on its inputs. That last detail is a good sign about who the API is being designed for: constant-time behavior is not something you retrofit.

## Dropping down to archsimd when the portable API is not enough

Every `simd` vector type has a `ToArch()` method returning `any`, and each type has a matching `simd.<Type>FromArch` constructor. Type-switch on the architecture-specific type, do the platform work, convert back. The Go blogâs worked example implements the missing `Int8s.OnesCount()` this way. On arm64 and wasm itâs short, because both have the instruction:

```
//go:build goexperiment.simd && (wasm || arm64)
package simd_test
import (
	"simd"
	"simd/archsimd"
)
// OnesCount returns the number of one bits for each element.
func OnesCount(v simd.Int8s) simd.Int8s {
	switch x := v.ToArch().(type) {
	case archsimd.Int8x16:
		return simd.Int8sFromArch(x.OnesCount())
	default:
		// GODEBUG=simd=0 emulation
		return OnesCountEmulated(v)
	}
}
```
The amd64 version runs longer: its 128- and 256-bit paths need a nibble lookup table, while the 512-bit path can call `OnesCount` directly. The cost model is the part that surprises people. An `any` conversion and a type switch in a hot path look expensive, but the compilerâs `simd` front end specializes the code and removes the switch.

The real price is maintenance. Take this escape hatch and you owe an implementation for every platform you build for, including a `!(amd64 || wasm || arm64)` fallback, forever, including the architectures Go adds after you stop paying attention. Stay inside the portable API unless the operation genuinely isnât expressible there.

## How specialization works, and why it shows up in your benchmarks

The `simd` package is three things: a package, an internal implementation package, and an AST rewrite in the compiler front end. The rewrite clones any function, variable, or type that mentions a `simd` type, swapping those types for size-specialized types in `simd/internal/bridge`. Clones get a suffix: `@simd128`, `@simd256`, `@simd512`, or `@simd0` for emulation. When one of those shows up in a stack trace, thatâs what youâre looking at.

Functions that use `simd` internally but donât mention it in their signature become wrappers that dispatch on the SIMD level detected at program start. Specialized functions call other specialized functions directly, so no dispatch happens inside a hot vector loop.

Which is fine until you benchmark. If your benchmark loop calls a SIMD function whose signature has no `simd` types, the dispatch lands inside the loop and you measure the wrapper. The fix in the Go blog is one unused declaration:

```
func BenchmarkVpsumdSIMD(b *testing.B) {
	// mention "simd" so the benchmark loop calls the specialized version directly
	var _ simd.Uint64s
	...
}
```
Mentioning a `simd` type makes the benchmark function itself a specialized function, which hoists the dispatch above the loop. Itâs a wart, and itâs the kind of wart that produces confidently wrong numbers in a blog post somewhere.

## Testing across SIMD levels without the hardware

`GODEBUG=simd=N` controls which vector width the runtime picks. It lets you exercise emulation and the widths compiled for the current architecture, but it does not make one architecture execute another architectureâs `archsimd` types:

- `simd=0` forces emulation even when hardware support exists. This is how you reach the`default` branches of your`ToArch` switches.
- `simd=128` selects 128-bit vectors and panics immediately if the required features are unavailable.`simd=256` and`simd=512` request those widths when possible.
- `simd=+128` ,`simd=+256` ,`simd=+512` select a width even when some features are missing. Code panics only when it actually executes an unsupported instruction. That models real hardware: a Raspberry Pi has NEON but no PMULL, and Apple Siliconâs amd64 emulation has AVX2 but no VPCLMULQDQ.

Wire these into a test matrix on day one. A `ToArch` switch that quietly falls through to emulation on one platform is otherwise invisible, and youâll find it as a performance mystery months later rather than a failing test. Same discipline as testing against other experimental Go packages like `testing/synctest`: the API will move between releases, and the tests are what tell you when it does.

For Go 1.28, the Go team intends to add SVE to `archsimd`, hopes to add it to `simd`, and plans more operations such as `OnesCount`, reductions, shuffles, and mask methods. Feature variants are also planned, so a machine with vectors but one missing instruction does not have to fall all the way back to emulation. If youâre starting now, write to the 1.27 intersection and keep a list of the places you wanted `ReduceSum` or a shuffle and worked around it. That list is your upgrade plan, and itâs cheaper than maintaining unnecessary `ToArch` switches.
