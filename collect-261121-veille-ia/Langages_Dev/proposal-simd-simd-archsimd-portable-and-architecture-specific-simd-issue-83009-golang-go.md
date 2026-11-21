---
id: collect-261121-veille-ia/langages-dev/proposal-simd-simd-archsimd-portable-and-architecture-specific-simd-issue-83009-golang-go
title: "proposal: simd, simd/archsimd: portable and architecture-specific SIMD · Issue #83009 · golang/go"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-20"]
keywords: ["simd", "archsimd", "equal", "across", "goexperiment", "vectors", "amd64", "architecture-specific"]
source: https://github.com/golang/go/issues/83009
source_anchor: ""
source_lines: [1, 166]
section: "Langages_Dev"
---
## Description

(Edited to fix formatting)

In Go 1.26, we introduced AMD64 architecture-specific SIMD intrinsics in the `simd/archsimd` package under GOEXPERIMENT=simd (#73787). In Go 1.27, we refined the AMD64 `simd/archsimd` API, expanded `simd/archsimd` to ARM64 NEON and WebAssembly SIMD, and introduced a portable, vector-size-agnostic SIMD API in the `simd` package (#78902), all still under GOEXPERIMENT=simd.

The detailed motivation and high level design approach are described in #73787.

We propose that a large subset of `simd` and `simd/archsimd` be graduated out of the GOEXPERIMENT, covering AMD64 (AVX, AVX2, and AVX512), ARM64 (NEON), and WebAssembly, with a software fallback of `simd` for other platforms.

Since Go 1.27, we’ve built an executable, pure-Go specification (#81819) of a large subset of the API that gives exact semantics for each operation, ensures consistency across architectures and both `simd` and `archsimd` (and forces us to be intentional *any* divergence in corner cases), and is now the source for uniform documentation across all covered operations.

We propose graduating exactly the operations covered by this specification:

The API is large, but highly structured, and we have several reasons to believe it’s quite robust at this point:

1. Both `simd` and`archsimd` follow the “one name means one thing” rule:*A name need not exist on every platform or in both packages, but wherever it does exist it must denote the same operation, with the same signature and the same semantics* . A method must mean one thing*across all types* within a category, where the only two categories are vectors and masks. Sticking to this rule across several platforms has kept the API very disciplined.
2. The specification enforces the “one name” rule, and we even use it for conformance testing. (In fact, I’ve found a few bugs in QEMU this way! Everything passes on real hardware.)
3. All operations in the specification use a custom type system designed for reasoning about shapes of strongly-typed vectors, including size-agnosticism.
4. While hardware SIMD ISAs often change radically across vector widths, nearly the entire API surface is vector-size-agnostic. We know this because of how it generalizes to the strictly vector-size-agnostic `simd` package, and because this property is captured in the spec’s vector type system.
5. While we’re not proposing GA for the ARM64 SVE API at this time, we do have a detailed prototype (simd/archsimd: support ARM64 SVE SIMD intrinsics under a GOEXPERIMENT #79781), which demonstrates that the API generalizes to true size-agnostic vector hardware.

We were nearly ready to propose general availability of the AMD64-specific API for Go 1.27 (#78979), but decided it wasn’t quite ready at the time. Now we have dramatically better evidence of convergence across platforms, and did in fact change several of the AMD64 APIs for better portability (notably, the conversion APIs).

## Open questions

Currently, we have `Vec.Equal(Vec) Mask`, which does an element-by-element equality comparison, but violates the standard convention of `Equal` methods returning `bool`. Furthermore, vector types currently disallow `==`: one perspective is that it feels a little too much like operator overloading; on the other hand, it’s completely reasonable to view vectors like `struct { a, b, c, d int }`, for which `==` would follow naturally. Options:

1. Leave it as is. Testing if two vectors are equal is `x.Equal(y).All()` . Testing if two masks are equal is`x.Xor(y).Not().All()` .
2. Leave element-wise `Vec.Equal(Vec) Mask` as is, but support`==` and`!=` on vectors and masks for boolean equality.
3. Rename the element-wise comparisons `Equal` →`Equals` ,`NotEqual` →`NotEquals` and add`Vec.Equal(Vec) bool` . This would require some staging to avoid needlessly breaking essentially all code using the GOEXPERIMENT: we’d first deprecate`Vec.Equal(Vec) Mask` and mark it as a`//go:fix` , leaving it only accessible under the GOEXPERIMENT; then a release later we’d remove this version of`Equal` and add the boolean`Equal` .

It’s often very helpful to be able to “reshape” between different vector shapes. Our current API for that is three steps: 1) one `Vec.ToBits() Vec2` method per vector type, where Vec2 is a `uint` vector with the same element width and lane count, 2) 4 `UintNxL.ReshapeToUintN’xL’` methods per `uint` vector, where N*L == N’*L’, and 3) 3 `UintNxL.BitsTo{Base}NxL` methods per `uint` vector. This keeps the reshaping API relatively small (though not super small), but is quite cumbersome to use. An alternative would be the introduce new type-less vector types `Bits{128,256,512}` and `Bits` (scalable) purely for reshaping purposes. Each vector type would have `ToBits()` to convert to the right `Bits*` type, and each `Bits*` type would have `To{Base}{N}x{L}` for each vector type of the same total width.

## Activity

1. Proposal review discussion: 
  - Equal: We don't say "AddElems", so it seems weird to call this anything other than "Equal". Also if we rename Equal, do we rename NotEqual, GreaterEqual, etc. Everyone seems okay with `==` because we allow it on structs and arrays and a vector is basically a struct or an array.
  - Reshaping: Isn't most "reasonable" reshaping only on uints anyway? It turns out people actually need a lot of surprising reshapings.
  - @cherrymui and @aclements have looked over the API very carefully. @adonovan has kindly offered to be a non-SIMD-project reviewer.
2. Equal: We don't say "AddElems", so it seems weird to call this anything other than "Equal". Also if we rename Equal, do we rename NotEqual, GreaterEqual, etc. Everyone seems okay with 
3. To me, `Equal` returning`Mask*` makes sense because`Mask*` is conceptually a vector of`bool` , and generally-speaking vector operations return a vector of whatever their result is.I assume the concern there is something around libraries like `github.com/google/go-cmp` which have a convention of sniffing for a`Equal() bool` method to provide a general, overloadable definition of equality?For that library in particular, It seems like an `Equal` method with the wrong signature would be ignored, and so current versions of that library would fall into the struct-comparing codepath and panic about the unexported fields as normal. Callers could use`AllowUnexported` to permit that in the short term. Since these types will be in the standard library, maybe a later version of that library would also learn to recognize the`Equal() simd.Mask` signatures, or to recognize the`simd` package's structs directly, and just start using`==` on them?
4. Minor naming inconsistency: For operations that only use part of their input or fill in part of their output, we follow the convention that it will always be the low elements. For example, converting a float32 vector to a float64 vector only uses the bottom half of its input vector. This is how most architectures behave, and for those that don't, we emulate by first doing the necessary permute. Because of this convention, we don't include "Lo" in method names *except in one case* :`MulWidenLo` . We should either drop`Lo` from`MulWidenLo` or add it to the names of operations that behave like this:Float32s.ConvertToFloat64s -> ConvertLoToFloat64s 
 ExtendTo*zE* -> ExtendLoTo*zE*I'm less concerned about the low-output names, but if we changed those, they would be Float64s.ConvertToFloat32s -> ConvertToFloat32sLo 
 TruncTo*zE* -> TruncTo*zE* Lo
 SaturateTo*zE* -> SaturateTo*zE* Lo
5. I've been reading the wasm side of the generator and ran into a few naming questions. Apologies if some of these are already settled somewhere I haven't found. `i32x4.dot_i16x8_s` looks like exactly`DotProductPairs` from`simd/internal/spec/math.go` , at E=int16, zE=int32. The doc comment there already calls out the only overflow case (all four inputs MinInt16) and agrees with the wasm spec that it wraps. Is it just not wired up yet, or is there something I'm missing?`extadd_pairwise_i8x16_s/u` might not need a name at all.`widen(x[2j]) + widen(x[2j+1])` is the same as`x[2j]*1 + x[2j+1]*1` , so it's`DotProductPairs(x, ones)` , which is also how the x86 backends get it. That seems like the same situation as`extend_high` , which the wasm generator maps to`"-"` with a note that it only exists for a fold. Would a fold be the preferred shape here too?`q15mulr_sat_s` looks like a genuine gap rather than a missing link. It's`sat_i16((x*y + 0x4000) >> 15)` , saturating only at x=y=-0x8000. I couldn't find a name for it in simd or archsimd on any platform. PMULHRSW is in the x86 encoding tables but isn't exposed anywhere. Is a Q15 rounding multiply in scope, or deliberately out?For the relaxed ops as a group, my reading is that they can't have spec entries at all: spec wants an executable reference implementation, and every relaxed op leaves some case unspecified. If that's the reasoning behind the `"-"` markers then that answers it and I'll stop poking at them.Last one and much smaller: there's a `relaxed_madd does not work` comment on the commented-out ternary float group. Known bug, or shorthand for the above?
6. ### 75 remaining items

## aclements commentedon Nov 11, 2026  

## apparentlymart commentedon Nov 11, 2026  

## aclements commentedon Nov 12, 2026  

This proposal has been added to the active column of the proposals project

and will now be reviewed at the weekly proposal review meetings.

— aclements for the proposal review group

## aclements commentedon Nov 13, 2026  

@apparentlymart , I assume

## aclements commentedon Nov 13, 2026  

## gopherbot commentedon Nov 15, 2026  

Change https://go.dev/cl/863945 mentions this issue:

`simd/archsimd/_gen/simdgen: split CPU feature checks by GOEXPERIMENT`
## gopherbot commentedon Nov 15, 2026  

Change https://go.dev/cl/863941 mentions this issue:

`internal/goexperiment: add simdstable experiment`
## gopherbot commentedon Nov 15, 2026  

Change https://go.dev/cl/863946 mentions this issue:

`simd/archsimd: move SVE feature checks behind GOEXPERIMENT=simd`
## iamrajiv commentedon Nov 15, 2026  

## gopherbot commentedon Nov 16, 2026  

Change https://go.dev/cl/863882 mentions this issue:

`simd: rename VectorBitSize to VectorBits`
simd: rename VectorBitSize to VectorBits

## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865829 mentions this issue:

`simd/archsimd, test: stop using deprecated As* and Merge internally`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865823 mentions this issue:

`simd/archsimd: delete dead testdata`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865828 mentions this issue:

`simd/archsimd/_gen/tmplgen: narrow with concatSelectedConstant`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865826 mentions this issue:

`cmd/api: print single-type constraints correctly`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865825 mentions this issue:

`simd: stop using HasHardwareCarrylessMultiply in tests`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865832 mentions this issue:

`simd/archsimd/_gen/simdgen: move experimental ops to ops_exp_gen_*.go`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865830 mentions this issue:

`simd/archsimd: move deprecated As* and Merge into deprecated_amd64.go`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865831 mentions this issue:

`simd/archsimd/_gen: stop generating As* intrinsics`
## gopherbot commentedon Nov 20, 2026  

Change https://go.dev/cl/865827 mentions this issue:

`simd/archsimd/_gen/midway: read only GA inputs and make checkSpec fatal`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863175 mentions this issue:

`simd/archsimd: use an unexported concatPermute128Scalars internally`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863178 mentions this issue:

`runtime: save vector registers on amd64 under GOEXPERIMENT_simdstable`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863176 mentions this issue:

`simd/archsimd: implement wasm IfElse with unexported bitSelect`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863177 mentions this issue:

`simd/archsimd: move experimental wasm ops to ops_exp_gen_wasm.go`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863180 mentions this issue:

`simd/archsimd: move cscimm4 and the _XXXX constants to consts_amd64.go`
## gopherbot commentedon Nov 21, 2026  

Change https://go.dev/cl/863179 mentions this issue:

`cmd/compile: gate fixed-width SIMD on Experiment.SIMDStable`to join this conversation on GitHub.Already have an account? Sign in to comment
