---
id: collect-261121-veille-ia/langages-dev/go-1-27-release-notes-breaking-changes-and-what-s-new
title: "Go 1.27 Release Notes: Breaking Changes and What's New"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["type", "func", "list", "metric", "celsius", "methods", "package-level", "sensor"]
source: https://levelupgo.dev/blog/whats-new-in-go-1-27
source_anchor: ""
source_lines: [1, 507]
section: "Langages_Dev"
---
Go 1.27 adds five new standard library packages (`encoding/json/v2`, `encoding/json/jsontext`, `crypto/mldsa`, `uuid`, and `simd`) plus three language changes. Some of it you'll want on day one, like generic methods and a `uuid` package that removes a dependency from most `go.mod` files.

Some of it will fail your test suite before you notice the feature exists:

- `compress/flate` emits different bytes
- Function literals get different symbol names
- A handful of GODEBUG escape hatches now fail the build instead of restoring old behavior

## Table of Contents

## Generic methods, and the one thing they still can't do

Since generics shipped in Go 1.18, type parameters have worked in exactly two places: top-level functions and type declarations. Methods were left out, so if you wanted a transform that changed the element type of your collection, you had to write it as a package-level function, and package-level functions don't chain.

You've probably written something like this. A metrics pipeline that turns sensor readings into display labels needs two transforms, and each one has to wrap the previous call:

example.gogo```
type Metric struct {
    Sensor  string
    Celsius int
}
type List[T any] []T
// Package-level, because a method couldn't declare U.
func Map[T, U any](l List[T], f func(T) U) List[U] {
    out := make(List[U], len(l))
    for i, v := range l {
        out[i] = f(v)
    }
    return out
}
func main() {
    metrics := List[Metric]{
        {Sensor: "cpu0", Celsius: 61},
        {Sensor: "cpu1", Celsius: 58},
    }
    temps := Map(metrics, func(m Metric) int { return m.Celsius })
    labels := Map(temps, func(c int) string { return fmt.Sprintf("%dC", c) })
    fmt.Println(labels) // [61C 58C]
}
```

In Go 1.27, `Map` becomes a real method that declares its own type parameter, independent of the receiver:

example.gogo```
// U is the method's own type parameter, new in Go 1.27.
func (l List[T]) Map[U any](f func(T) U) List[U] {
    out := make(List[U], len(l))
    for i, v := range l {
        out[i] = f(v)
    }
    return out
}
func main() {
    metrics := List[Metric]{
        {Sensor: "cpu0", Celsius: 61},
        {Sensor: "cpu1", Celsius: 58},
    }
    labels := metrics.
        Map(func(m Metric) int { return m.Celsius }).
        Map(func(c int) string { return fmt.Sprintf("%dC", c) })
    fmt.Println(labels) // [61C 58C]
}
```

Chaining is the obvious improvement. Naming is the less obvious one: `MapSlice`, `MapSet`, and `MapBox` collapse into a single `Map` per type, because the receiver already says which type you mean.

The receiver helps the compiler too: `List[Metric]` pins `T`, so only the output type is left to infer. And methods show up when you type a dot in your editor. A package-level helper only helps if you already know it exists.

The standard library uses the feature itself. `math/rand/v2` now declares `(*Rand) N[Int intType](Int) Int` as a method alongside the package-level `N` function, which the old rules didn't allow.

There is a limit, and it's easy to miss. From the release notes:

So this does not compile:

example.gogo```
type Transformer interface {
    // Compile error: interface methods can't declare type parameters.
    Map[U any](f func(Metric) U) any
}
```

Interface dispatch happens at runtime, so the compiler can't know which instantiations of a generic method to generate for a dynamically dispatched call. If your API is built on an interface, transforms still need the old package-level function. Whether generic methods help your codebase mostly comes down to this, so check your interfaces before you plan a refactor.

If you want the reps, the Go Generics Masterclass covers constraints and inference as in-browser exercises.

**Verdict: adopt now**, for concrete types. Leave interface-shaped APIs alone.

## A uuid package in the standard library

Almost every Go service that talks to a database imports `github.com/google/uuid`. Go 1.27 adds a standard library package with the import path `"uuid"`, following proposal #62026.

`UUID` is defined as `[16]byte`, which means values are comparable with `==` and usable as map keys directly. Three generators ship:

example.gogo```
package main
import (
    "fmt"
    "uuid"
)
func main() {
    fmt.Println(uuid.New())   // reach for this when you don't care how it's made
    fmt.Println(uuid.NewV4()) // 128 bits, 122 of them random
    fmt.Println(uuid.NewV7()) // 48-bit timestamp first, so values sort by creation time
    requestID, err := uuid.Parse("f81d4fae-7dec-11d0-a765-00a0c91e6bf6")
    if err != nil {
        return
    }
    fmt.Println(requestID, requestID == uuid.Nil()) // false, it parsed fine
}
```

`New` is the default and currently returns a V4. `NewV4` is fully random, so nobody can guess the next one. That's what you want in a public request ID.

`NewV7` puts a 48-bit timestamp up front, so newer values sort after older ones, which makes them good primary keys because inserts land near the end of the index instead of all over it.

Note that `Nil()` and `Max()` are functions, so the comparison is `id == uuid.Nil()` with parentheses. `Nil()` works as a "not set yet" sentinel. `Max()` is the all-ones value, which is useful as the upper bound when you scan a range of time-ordered V7 keys.

**Verdict: adopt now** for new code. Migrating an existing service is a find-and-replace, but see the FAQ before you delete the dependency.

## Do you need to change anything for encoding/json/v2?

No. That was the point of the design. `encoding/json` is now backed by the v2 implementation, and the release notes are explicit that "Marshaling and unmarshaling behavior is preserved, but the exact text of error messages may differ."

You get the new engine's speed without changing any code. Per the release notes, "Marshal performance is broadly at parity with the previous implementation, while unmarshal performance is significantly faster."

You only opt into new *behavior* by importing `encoding/json/v2` by name. That package has stricter defaults, chosen to match what other JSON implementations expect. It:

- Rejects invalid UTF-8 in JSON strings
- Rejects duplicate names within a JSON object
- Matches field names case-sensitively
- Marshals a nil slice as `[]` rather than`null`
- Marshals maps in a non-deterministic order where v1 guaranteed a deterministic one

The nil-slice change is the nil-versus-empty distinction from var vs make in Go, and under v2 you can see it in the output.

example.gogo```
import "encoding/json/v2"
type Event struct {
    ID     string `json:"id"`
    Action string `json:"action"`
}
// v1 would match the key "ID" against the tag `json:"id"`. v2 won't,
// and it skips the mismatched key silently rather than erroring.
var e Event
json.Unmarshal([]byte(`{"ID":"evt_01H","action":"checkout"}`), &e)
fmt.Println(e.ID, e.Action) // "" checkout
// v2 marshals maps in a non-deterministic order. Ask for a stable one
// explicitly when you hash or snapshot the output.
inventory := map[string]int{"widget": 12, "gadget": 3, "gizmo": 7}
b, _ := json.Marshal(inventory, json.Deterministic(true))
```

To get a v1 behavior back under v2, you don't switch imports. You pass an option for that one behavior:

- `Deterministic(true)` for stable map order
- `MatchCaseInsensitiveNames(true)` for loose field matching
- `FormatNilSliceAsNull(true)` for`null` instead of`[]`

The v1 package gained the same options, so you can adopt v2 semantics one behavior at a time without a full migration. The complete list lives under Migrating to v2 in the v1 package docs.

The third package, `encoding/json/jsontext`, handles the lower-level syntax. It exposes JSON as a sequence of tokens and values, with a state machine that keeps the output valid. Streaming codecs build on it. Most application code never imports it directly.

To decide which one to import:

```
flowchart TD
    A["Which JSON import?"] --> B{"Upgrading existing code?"}
    B -- Yes --> C["encoding/json<br/>v2 engine, v1 behavior"]
    B -- No --> D{"Want strict defaults<br/>and faster decoding?"}
    D -- No --> C
    D -- Yes --> E["encoding/json/v2"]
    E --> F{"Need raw tokens<br/>or streaming syntax?"}
    F -- Yes --> G["encoding/json/jsontext"]
    F -- No --> H["You're done"]
```

*Decision tree for choosing between `encoding/json`, `encoding/json/v2`, and `encoding/json/jsontext` in Go 1.27.*

**Verdict: adopt the default now** (you already have), **adopt v2 by name later**, once you've read the migration notes for your struct tags. The Data Formats course covers the tag rules that decide how much of this affects you.

## Post-quantum signatures with crypto/mldsa

RSA and ECDSA are safe today because no computer can factor large numbers or solve discrete logs fast enough. A large enough quantum computer could. `crypto/mldsa` implements ML-DSA from FIPS 204 in three parameter sets, 44, 65, and 87, following proposal #77626.

Other packages already support it. `crypto/x509` parses and verifies ML-DSA keys and signatures, and `crypto/tls` accepts all three parameter sets in a TLS 1.3 handshake. `MLKEM1024` joins the supported key exchanges for quantum-safe key agreement, enabled by adding it to `Config.CurvePreferences`.

A full sign-and-verify for a firmware release:

example.gogo```
sk, err := mldsa.GenerateKey(mldsa.MLDSA65())
if err != nil {
    return err
}
firmware, err := os.ReadFile("firmware-v2.1.0.bin")
if err != nil {
    return err
}
// Context is a domain-separation label. Sign and verify must pass the same one.
opts := &mldsa.Options{Context: "acme/firmware-release"}
sig, err := sk.Sign(nil, firmware, opts)
if err != nil {
    return err
}
fmt.Println(mldsa.Verify(sk.PublicKey(), firmware, sig, opts) == nil) // true
fmt.Println(mldsa.MLDSA65().SignatureSize())                         // 3309
firmware[0] ^= 1 // flip one bit
fmt.Println(mldsa.Verify(sk.PublicKey(), firmware, sig, opts) == nil) // false
```

`Verify` returns `nil` when the signature is good, so comparing against `nil` gives you a boolean. The downside is size, and the difference is large:

*Source: signature sizes per FIPS 204 (ML-DSA) and the respective algorithm specifications.*

An ML-DSA-65 signature is about 46 times the size of an Ed25519 one, so it's worth using only where the signature has to last.

Firmware and release artifacts that a device must still trust in ten years are worth 3 KB. So are certificate roots meant to outlive today's crypto, and TLS servers you'd rather not re-key once quantum computers show up. For short-lived session tokens that you mint thousands of per second, the extra size is overhead you don't need yet.

One unrelated `crypto/x509` change lands in the same area. `SystemCertPool` now respects `SSL_CERT_FILE` and `SSL_CERT_DIR` on Windows and Darwin, not just Linux. When those are set, Go loads roots from disk and uses its own verifier instead of the platform APIs. Set `GODEBUG=x509sslcertoverrideplatform=0` to keep the old behavior.

**Verdict: adopt for long-lived signatures, ignore for everything else.**

## The experimental simd package

A normal CPU instruction works on one value. A SIMD instruction works on a whole vector of values in one step, so the same result takes far fewer instructions.

That helps when you run the same math over a large slice of audio samples or image pixels, or compute a dot product. Code that mostly shuffles structs and strings won't benefit.

Each architecture exposes SIMD differently. The new `simd` package is portable and doesn't assume a vector size. It's available on all architectures and uses hardware instructions where they exist. Enable it with `GOEXPERIMENT=simd` at build time. It's experimental, so the API can change.

Mixing two audio tracks, several samples per step:

example.gogo```
//go:build goexperiment.simd
// out[i] = trackA[i] + trackB[i], several samples at a time.
out := make([]float32, len(trackA))
lanes := simd.LoadFloat32s(trackA).Len() // how many float32s fit in one vector
for i := 0; i+lanes <= len(trackA); i += lanes {
    va := simd.LoadFloat32s(trackA[i:])
    vb := simd.LoadFloat32s(trackB[i:])
    va.Add(vb).Store(out[i:])
}
// A plain scalar loop handles the leftover tail.
```

`Len` reports how many elements fit in one vector on the machine you're running on, so the loop never has to hardcode a vector width. Each step loads that many samples from both tracks, adds them element-wise in one operation, and stores the result.

**Verdict: ignore for now** unless you already profile a numeric hot loop. The API is behind a GOEXPERIMENT and subject to change.

## Performance you get just by upgrading

Some speedups need nothing but a rebuild. The biggest one is size-specialized allocation, described in the release notes this way:

*Source: Go 1.27 release notes, faster memory allocation.*

Those two numbers measure different things. The 30% is the cost of a single small allocation call. The ~1% is what a whole service sees. If you hit a regression, `GOEXPERIMENT=nosizespecializedmalloc` opts out, and that escape hatch is expected to disappear in Go 1.28.

Three compiler optimizations are also on by default:

- A known-bits dataflow pass tracks which bits of a value are provably 0 or 1 and folds away the resulting redundancy.
- Loop-invariant code motion lifts computations whose result never changes out of the loop body, so they run once instead of every iteration.
- Switch statements now compile to lookup tables where the cases allow it, including with `fallthrough` , jumping straight to the matching case instead of testing each one.

`compress/flate` gets faster too, with one caveat: the exact encoded output may differ from Go 1.26. DEFLATE sits underneath `archive/zip`, `compress/gzip`, `compress/zlib`, and `image/png`, so byte-exact golden tests over any of them can fail. The output is still correct, so re-bless the fixtures.

**Verdict: adopt now.** You get it on upgrade either way.

## Finding goroutine leaks and reading labeled tracebacks

The `goroutineleak` profile graduated from experiment to generally available, and the `goroutineleakprofile` GOEXPERIMENT is deleted. It's exposed through `runtime/pprof` and as the `net/http/pprof` endpoint `/debug/pprof/goroutineleak`.

A leaked goroutine is one blocked on a concurrency primitive that can never unblock. The runtime finds them through the garbage collector: if goroutine G is blocked on primitive P, and P is unreachable from any runnable goroutine or anything those could unblock, then G can never wake up.

example.gogo```
func startJob() {
    result := make(chan int) // unbuffered: the send waits for a receiver
    go func() {
        result <- expensiveWork() // blocks forever, nobody receives
    }()
    // returns without receiving, so result becomes unreachable
}
func main() {
    startJob()
    runtime.GC() // the detector scans during a GC cycle
    pprof.Lookup("goroutineleak").WriteTo(os.Stdout, 1)
    // goroutineleak profile: total 1
    //   ... main.startJob.func1 ... main.go:14
}
```

This approach has a blind spot. The runtime can miss leaks where the blocking primitive is reachable through a global variable or through the locals of a runnable goroutine. A channel stored in a package-level registry counts as reachable, so the profile won't report it.

Concurrency Fundamentals walks through the leak patterns that produce these, and how to close them with context.

The second debugging improvement needs no code at all. For modules whose `go` directive is 1.27 or later, tracebacks now include `runtime/pprof` goroutine labels in the header line. The labels you already set for profiling show up in crash dumps, `SIGQUIT` traces, and `runtime.Stack` output. When two goroutines have identical stacks, the label is often the only way to tell them apart:

example.gogo```
pprof.Do(ctx, pprof.Labels("request", id), func(ctx context.Context) {
    work()
})
// goroutine 34 [running]:
// labels: {"request":"req-42"}
// main.handle.func1 ...
```

Set `GODEBUG=tracebacklabels=0` to turn it off if your labels carry anything you don't want in a crash dump.

**Verdict: adopt now.** Neither one costs you anything.

## Smaller language and library wins

### Struct-literal field selectors

A key in a struct literal may now be any valid field selector for the type, not just a top-level field name (proposal #9859). If you embed a shared model, you no longer have to spell out the nested literal:

example.gogo```
type Model struct {
    ID        int64
    CreatedAt time.Time
}
type Post struct {
    Model
    Author string
    Likes  int
}
p := Post{Model: Model{ID: 42}, Author: "Patrik"} // before
p := Post{ID: 42, Author: "Patrik"}               // Go 1.27
```

### Generalized function type inference

Function type inference now applies in every context where a generic function is assigned to, or converted to, a matching function type (proposal #77245). Previously it only worked in a typed variable declaration:

example.gogo```
func ascending[T cmp.Ordered](a, b T) int  { return cmp.Compare(a, b) }
func descending[T cmp.Ordered](a, b T) int { return cmp.Compare(b, a) }
orders := []func(int, int) int{ascending[int], descending[int]} // before
orders := []func(int, int) int{ascending, descending}           // Go 1.27
```

The slice's element type is `func(int, int) int`, so Go infers `T` as `int`. The same now works for conversions and for passing a bare generic function as an argument.

### strings.CutLast and bytes.CutLast

`Cut` splits on the first separator. `CutLast` splits on the last, returning the part before, the part after, and whether the separator was found (proposal #71151):

example.gogo```
dir, file, found := strings.CutLast("internal/store/user.go", "/")
fmt.Println(dir, file, found) // internal/store user.go true
```

Before, you'd call `LastIndex`, check for `-1`, and slice twice by hand.

### Rand.N, maphash.Hasher, and big.Int.Divide

`math/rand/v2` gets `Rand.N` as a generic method (proposal #77853), so you can draw a bounded random value from a source you seeded instead of the global one, which can't be seeded. A failing test then replays with the same inputs.

`hash/maphash` gains a generic `Hasher[T]` interface for hash-based structures such as custom maps and Bloom filters (proposal #70471). It pairs a hash function with an equality check, and equal values must produce the same hash. `ComparableHasher[T]` is the ready-made implementation for comparable types that compares with `==`. `go/types` already uses it, shipping a `Hasher` that respects the `Identical` relation.

`math/big.Int` gets a `Divide` method that computes quotient and remainder together under a rounding mode you choose: `Trunc`, `Floor`, `Round`, or `Ceil`. `Quo` and `Mod` always truncate toward zero, so financial and numeric code that needs other rounding now has a built-in option.

### Unicode 17, database/sql, and the rest

The `unicode` package and everything built on it moved from Unicode 15 straight to Unicode 17, so characters added in the two intervening releases now classify correctly. `database/sql` gained `ConvertAssign`, which lets drivers reuse the type conversions `Rows.Scan` performs instead of writing their own.

The rest are small:

- `crypto` adds the`MLDSAMu` hash value, a signaling mechanism for external-mu ML-DSA signing.
- `crypto/ecdsa` now checks that the hash length is correct in`PrivateKey.Sign` when you pass non-nil`SignerOpts` .
- `crypto/x509` exposes`RawSignatureAlgorithm` on`Certificate` ,`CertificateRequest` , and`RevocationList` , giving you the DER-encoded AlgorithmIdentifier even when`SignatureAlgorithm` is`UnknownSignatureAlgorithm` . Parsing into`pkix.Name` also accepts a wider range of value types, with unknown ones landing in`asn1.RawValue` .
- `crypto/tls` adds`ConnectionState.LocalCertificate` , the chain you presented to the peer, plus`QUICConfig.ClientHelloInfoConn` .`Config.Rand` is deprecated in favor of`testing/cryptotest.SetGlobalRandom` for deterministic tests.
- `net` makes`UnixConn` read methods return`io.EOF` directly instead of wrapping it in a`net.OpError` .
- `runtime/secret` propagates secret mode into goroutines created inside it.
- `go/constant` adds`StringLen` ,`go/scanner` adds`Scanner.End` , and`go/token` gives`File` a`String` method.
- On the ports side, the big-endian 64-bit PowerPC port on Linux moves to the ELFv2 system ABI, which unlocks cgo, position-independent executables, and external linking there, and needs a Linux 3.13 kernel or later. On Plan 9, `syscall.Errno` is now defined and implements`error` , so portable code referring to it builds without build constraints. The linker also accepts`-macos` and`-macsdk` to set the versions written into the macOS`LC_BUILD_VERSION` load command.

## What changed in the Go toolchain?

`go fix` gained four modernizers:

- `embedlit` simplifies references to embedded fields in composite literals, which the new struct-literal rule makes possible.
- `atomictypes` replaces basic types in`sync/atomic` calls with atomic types.
- `slicesbackward` rewrites backward loops into`slices.Backward` .
- `unsafefuncs` replaces unsafe pointer arithmetic with function calls.

Two housekeeping changes came with them: `fmtappendf` was removed over stylistic concerns, and `waitgroup` was renamed to `waitgroupgo`. Run it once after you bump the `go` directive:

example.bashbash```
go fix -diff ./...   # preview
go fix ./...         # apply
```

For the full catalog of what `go fix` rewrites, see every modernizer in go fix 1.26.

`go test` now runs the `stdversion` vet check by default. It reports standard library symbols that are newer than the Go version in force for the file, as set by your `go.mod` directive and build tags. If your module declares `go 1.25` and someone uses `strings.CutLast`, the test run catches it before a user on 1.25 does.

`go test -json` also annotates `"Action":"output"` lines with an optional `"OutputType"` field, currently `error`, `error-continue`, or `frame`, which is useful if you parse test output in CI.

`go doc` accepts `package@version` syntax now, so `go doc rsc.io/[email protected]` reads the docs for an exact release without checking it out. A new `-ex` flag lists a package's runnable examples, and naming one prints its source.

`go mod tidy` enforces a two-block layout for modules on `go 1.27` or later, merging scattered `require` blocks into one direct and one indirect block. Comment blocks attached to dependencies are preserved, and a comment spanning a mixed set moves to the direct block.

`go tool trace -http=:6060` now binds to localhost only, matching `go tool pprof`, so pass a full address like `-http=0.0.0.0:6060` if you need it reachable from another machine. The `compile`, `link`, `asm`, `cgo`, `cover`, and `pack` tools also accept response files (`@file`) in a GCC-compatible format, which helps build systems that exceed command-line length limits.

## Testing and net/http

`httptest.NewTestServer` creates a `Server` on an in-memory fake network, built for use with `testing/synctest`, so there's no real TCP port involved. Its partner is `synctest.Sleep`, which does `time.Sleep` and `synctest.Wait` in one call, advancing the fake clock and then letting goroutines settle. With both, HTTP tests stop depending on timing. Professional Go Testing covers the `synctest` model these build on.

example.gogo`srv := httptest.NewTestServer(t, handler) // in-memory, cleanup auto-registered`

On the `net/http` side, the HTTP/2 server now accepts client priority signals as defined in RFC 9218 and serves higher-priority streams first. Set `Server.DisableClientPriority = true` for the old round-robin behavior.

The change you're most likely to notice is on HTTP/1: closing a partially-read `Response.Body` now drains the remaining content up to a conservative limit so the connection can be reused. For most programs that's a no-op or a small speedup. If you close early to abort a large download, `Transport.DisableKeepAlives = true` turns it off.

- `Server.MaxHeaderValueCount` caps how many values a single header may carry, which guards against requests that flood a header.
- `Transport` and`Server` can negotiate TLS ALPN on a`net.Conn` you supply yourself, as long as it implements`ConnectionState() tls.ConnectionState` , so HTTP/2 gets picked up over tunneled or proxied connections.
- `net/url` gained`URL.Clone` and`Values.Clone` for deep copies. HTTP and Networking covers the server and transport settings these build on.

**Verdict: adopt now.** If you have a flaky HTTP test suite, `httptest` plus `synctest` is reason enough to upgrade.

## What breaks when you upgrade to Go 1.27?

The release notes spread the breaking changes across eight sections. This table collects them, ordered by how likely you are to hit them.

| What changes | Who it bites | What to do | 
|---|---|---|
| `compress/flate` emits different bytes | Byte-exact golden tests over gzip, zlib, zip, or png output | Re-bless the fixtures. Compression is correct, just different. | 
| Simpler function-literal (closure) names | Tests asserting on symbol names, and code comparing function code pointers for equality | Stop depending on literal names. Pointer comparison of funcs was already documented as unreliable. | 
| `asynctimerchan` and`gotypesalias` GODEBUGs removed permanently | Anything pinning them to the old value in `go.mod` or a`//go:debug` line | `grep -r asynctimerchan .` before upgrading. Setting the final default still builds, setting the old value fails. | 
| Five TLS/x509 GODEBUGs removed: `tlsunsafeekm` ,`tlsrsakex` ,`tls3des` ,`tls10server` ,`x509keypairleaf` | Services still pinned to legacy TLS behavior | Same rule as above: the `go` command accepts the final default and rejects the old value, so you find these at build time. | 
| macOS 13 Ventura is the minimum | CI runners and dev machines on older macOS | Bump the runner image. Announced in the Go 1.26 notes. | 
| `bzr` support removed from the`go` command | Modules hosted on Bazaar servers | Mirror the dependency, or vendor it. | 
| New `//go:linknamestd` directive marks std-only linknames, the linker now checks linkname access to assembly symbols, and type descriptors moved into a`.go.type` section | Packages reaching into the runtime through an unsanctioned `//go:linkname` , yours or a dependency's | Update the dependency. These fail at build time, loudly. None of it is in the release notes, so a build is how you find out. | 
| HTTP/1 `Response.Body.Close` drains unread content | Code that closes early to abort a large download | Set `Transport.DisableKeepAlives = true` for those clients. | 
| json/v2 error message text differs | Tests asserting on exact JSON error strings | Match on error type or a substring instead of the full message. | 

The last row is the hardest to spot. The behavior is preserved but the text isn't, so a test suite that string-matches unmarshal errors fails with no obvious cause. Isolate it with two runs on a branch that has already bumped `go.mod` to `go 1.27`:

example.bashbash```
go test ./... > default.txt 2>&1
GOEXPERIMENT=nojsonv2 go test ./... > nojsonv2.txt 2>&1
diff default.txt nojsonv2.txt
```

Anything in the diff comes from the JSON change. Anything failing in both runs is something else in the table. `GOEXPERIMENT=nojsonv2` is expected to be removed in a future release, so use it to diagnose and then fix the tests.

## Go 1.27 point releases: 1.27.1 to 1.27.4

The latest release is Go 1.27.4, from November 10, 2026. Point releases carry only security and bug fixes, with no language changes or new APIs, so moving from 1.27.0 to the newest one is a safe upgrade. Three of the four include security fixes.

| Release | Date | What it fixes | 
|---|---|---|
| Go 1.27.4 | 2026-11-10 | Security fixes to cgo, the `go` command, the compiler, the linker, and`net/http` . Bug fixes to the assembler, compiler, runtime, toolchain bootstrapping, and`crypto/fips140` ,`crypto/hkdf` ,`crypto/rsa` ,`crypto/tls` , and`net` . | 
| Go 1.27.3 | 2026-10-19 | Security fixes to `crypto/x509` . | 
| Go 1.27.2 | 2026-10-08 | Security fixes to the `go` command,`crypto/tls` ,`html/template` ,`net/http` ,`net/textproto` , and`os` . Bug fixes to the compiler, linker, runtime,`cover` ,`go fix` , vet, and`compress/flate` ,`crypto/mlkem` ,`encoding/json` ,`encoding/json/v2` ,`net/http` , and`os` . | 
| Go 1.27.1 | 2026-09-01 | Bug fixes to cgo, the compiler, the runtime, `go fix` , and`database/sql` ,`debug/elf` ,`encoding/json` ,`net/http` ,`os` ,`simd` , and`simd/archsimd` . | 
| Go 1.27.0 | 2026-08-19 | The major release covered in the rest of this post. | 

Go 1.26 got the same security fixes on the same days, as 1.26.9 to 1.26.11. Upgrading within 1.26 is the quicker path if you aren't ready for the breaking changes above. To build a module with the newest patch, run `go get [email protected]`. That sets the `toolchain` line in `go.mod` without raising the `go` line your dependents must meet. Then rerun your tests.

## Frequently Asked Questions

### Do I have to rewrite my JSON code for Go 1.27?

No. The `encoding/json` package is now implemented on top of v2, but marshaling and unmarshaling behavior is preserved and the v1 API stays supported. The only observable difference is that the exact text of error messages may change. You opt into the stricter v2 defaults only by importing `encoding/json/v2` explicitly.

### Can a generic method satisfy an interface?

No. The release notes state that interface methods may not declare type parameters, and that interface methods cannot be implemented by generic methods. Interface dispatch resolves at runtime, so the compiler can't know which instantiations to generate for a dynamically dispatched call. If your API is built on interfaces, keep using package-level generic functions.

### Should I drop github.com/google/uuid for the standard library package?

For new code, yes. For existing code, check two things first. The standard library `UUID` is `[16]byte` with its own method set, so any code touching the third-party type's extra methods needs review, and any dependency that exports the third-party `uuid.UUID` in its API keeps that module in your graph regardless.

### Is crypto/mldsa ready for production?

It's a stable standard library package implementing FIPS 204, wired into `crypto/x509` and `crypto/tls`. The practical constraint is signature size, not maturity: ML-DSA-65 signatures are 3309 bytes against 64 for Ed25519. Use it where the signature must outlive today's cryptography, and skip it for high-volume short-lived tokens.

### What is the fastest way to find what Go 1.27 breaks in my codebase?

Bump `go.mod` to `go 1.27` on a branch, then grep for `asynctimerchan` and `gotypesalias` and the five removed TLS settings, since those fail the build outright. Then run your suite twice, once normally and once with `GOEXPERIMENT=nojsonv2`, and diff the failures. That separates JSON error-text drift from golden-file and symbol-name failures.

### What is the latest Go 1.27 point release?

Go 1.27.4, released November 10, 2026. It has security fixes to cgo, the `go` command, the compiler, the linker, and `net/http`. Go 1.27.2 and 1.27.3 also had security fixes, so anything still on 1.27.0 or 1.27.1 should upgrade. Point releases add no new features and no breaking changes.

## Sources

Primary references cited in this post (last verified August 23, 2026):

- Go 1.27 Release Notes, retrieved 2026-08-23
- Go release history (point releases), retrieved 2026-11-17
- Go 1.27 interactive tour, VictoriaMetrics, retrieved 2026-08-23
- `uuid` package documentation, retrieved 2026-08-23
- `encoding/json/v2` package documentation, retrieved 2026-08-23
- `encoding/json` : Migrating to v2, the complete list of behavioral differences, retrieved 2026-08-23
- `crypto/mldsa` package documentation, retrieved 2026-08-23
- `simd` package documentation, retrieved 2026-08-23
- `net/http/httptest` package documentation, retrieved 2026-08-23
- NIST FIPS 204: Module-Lattice-Based Digital Signature Standard, retrieved 2026-08-23
- `x/tools` modernize analyzer documentation, retrieved 2026-08-23
- Unicode 17.0.0 release notes, retrieved 2026-08-23

## Keep going

Generic methods are easier to use well once constraints and inference feel natural, and those are the parts people tend to skim when they first learn generics. The Go Generics Masterclass works through constraints, type sets, and inference as in-browser exercises that run on the current toolchain, so you can try the 1.27 method syntax in real code.

Newer to Go? Start with the Go Fundamentals track and come back for the release notes. And if you skipped last year's release, What's New in Go 1.26 covers `errors.AsType`, the Green Tea garbage collector, and `new(expr)`.
