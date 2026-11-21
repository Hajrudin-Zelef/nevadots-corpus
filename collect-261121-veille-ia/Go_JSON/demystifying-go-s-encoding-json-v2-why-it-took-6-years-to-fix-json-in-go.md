---
id: collect-261121-veille-ia/go-json/demystifying-go-s-encoding-json-v2-why-it-took-6-years-to-fix-json-in-go
title: "Demystifying Go's encoding/json/v2: Why It Took 6 Years to Fix JSON in Go"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["json", "encoding", "years", "utf-8", "struct", "half", "every", "took"]
source: https://jadmadi.net/posts/go-json-v2-explained/
source_anchor: ""
source_lines: [1, 435]
section: "Go_JSON"
---
Do you also feel a quiet sense of dread when a simple Go struct needs custom JSON serialization? You are not alone.

Go has shipped `encoding/json` in the standard library since before Go 1.0,
more than fifteen years ago. For a decade and a half, it worked well enough
that every Go service on the internet used it, and badly enough that
Kubernetes, Protocol Buffers, and half the cloud ecosystem had to write grim
workarounds around it.

On November 4, 2026, the Go team announced that `encoding/json/v2` and its
companion package `encoding/json/jsontext` are officially stable and generally
available in Go 1.27<sup>1</sup>. The effort took six years, starting from Daniel
MartÃâs late-2020 design notes and Joe Tsaiâs frustration that `protojson`
could not even use the standard library to serialize Protocol Buffers.

The pitch is cleaner semantics and up to 10x faster unmarshaling. The catch is
that if you treat `json/v2` as just a drop-in import path change, you will miss
the architectural shift that actually unlocks those gains.

Letâs walk through what broke in `v1`, how `jsontext` and `json/v2` split the
problem in half, and what changes in your code. Not the release note summary.
The mechanism.

**Whatâs in this post:**

## The One Rule

Here is the rule, and I want you to hold it for the whole post:

**JSON processing is two different jobs: grammar and meaning. `v1` fused them
together; `v2` splits them apart.**

Every time a program reads or writes JSON, two distinct layers are at work:

1. **Syntactic processing (encode / decode):** Validating UTF-8, matching
braces, reading tokens, and slicing raw JSON values according to the RFC
8259 grammar. This job knows nothing about Go structs, reflection, or tags.
2. **Semantic processing (marshal / unmarshal):** Translating those syntactic
tokens into Go structs, maps, slices, and`time.Time` values using Goâs type
system.

In `encoding/json` (`v1`), syntax and semantics were tangled inside the same
types. `json.NewDecoder` sounded like a syntactic stream reader, except it
buffered entire values in memory and exposed semantic struct options that
vanished the moment a nested field called a custom `UnmarshalJSON([]byte)`
method.

Notice what happens when you separate the two layers. The bottom layer
(`encoding/json/jsontext`) becomes a pure, reflection-free streaming engine for
RFC 8259 grammar. The top layer (`encoding/json/v2`) becomes a clean semantic
mapper built on top of that stream.

Hold on to this rule. Every fix in `json/v2` flows directly from it.

## The Five Behavioral Traps That `v1` Could Never Fix

The dominant narrative around `encoding/json` was always that it was âa bit
slow because of reflection.â But when you look at what actually bit production
systems over the last fifteen years, reflection speed was only half the story.
The deeper problem was that `v1` shipped before modern JSON interoperability
hardened around RFC 8259, and the Go 1 compatibility promise froze
every early mistake in amber.

Here are the five behavioral flaws that forced a new major version:

- **Silent UTF-8 corruption:** RFC 8259 requires valid UTF-8.`v1` silently
accepts invalid UTF-8 bytes and replaces them with the Unicode replacement
character (`U+FFFD` ), corrupting data downstream without returning an error.`v2` rejects invalid UTF-8 by default.
- **Duplicate object keys (the `CVE-2017-12635` trap):** What does`{"role": "user", "role": "admin"}` mean? RFC 8259 leaves duplicate member
names unspecified. One parser takes the first key; another takes the last; a
third merges them. Attackers exploit that disagreement to bypass policy
checks between proxies and backend services.`v1` silently accepts duplicate
keys.`v2` rejects duplicate names by default.
- **Case-insensitive unmarshaling:** When`v1` unmarshals a JSON object into a
Go struct, it falls back to case-insensitive field matching. If your JSON has`"user_id"` or`"USERID"` ,`v1` scans struct fields with`strings.EqualFold` .
That default is slow, surprising, and a security footgun when untrusted payloads
shadow canonical fields.`v2` uses strict case-sensitive matching by default.
- **Leaking Go `nil` slices and maps as `null`:** In Go, an uninitialized`var items []string` is`nil` , and appending to it works without allocation.
Yet`v1` marshals a`nil` slice as`null` and an empty slice`[]string{}` as`[]` . Every frontend developer who has ever seen a TypeScript client crash on`response.items.map(...)` has paid the tax for this leak. Following a broad Go
community survey,`v2` marshals`nil` slices as`[]` and`nil` maps as`{}` by
default.
- **Broken `omitempty` and pointer receiver inconsistency:** In`v1` ,`omitempty` meant âomit if the Go value is a zero value,â which failed to
omit empty structs (`struct{}{}` ) while unexpectedly omitting booleans or
numbers. Worse, a`MarshalJSON` method defined on a pointer receiver
(`func (t *MyType) MarshalJSON()` ) was called inconsistently depending on
whether the value lived inside an interface or a struct value.`v2` redefines`omitempty` around*empty JSON output* (`null` ,`""` ,`[]` ,`{}` ), pairs it
with`omitzero` for Go zero values, and invokes methods consistently.

And then there is the classic `io.Reader` trap in `v1`. Millions of lines of Go
contain this idiom:

```
// In encoding/json (v1): reads one JSON value and silently
// ignores any trailing garbage left in the io.Reader!
err := json.NewDecoder(r).Decode(&payload)
```
Because `json.Decoder` in `v1` was designed for concatenated streams, `Decode`
stops after the first valid JSON value and never verifies EOF. If an attacker
or broken client sends `{"ok":true}THIS_IS_GARBAGE`, `Decode` returns `nil`.

## Side-by-Side Cheat Sheet: `v1` vs `v2`

If you want the operational differences on a single screen, here is what
changes when you switch from `encoding/json` to `encoding/json/v2`:

| Behavior / Feature | `encoding/json` (`v1` ) | `encoding/json/v2` (`v2` ) | 
|---|---|---|
| **Invalid UTF-8** | Silently coerces to `U+FFFD` | Returns an error by default | 
| **Duplicate JSON keys** | Silently overwrites / merges | Returns an error by default | 
| **Struct field matching** | Case-insensitive fallback | Strict case-sensitive default | 
| **`nil` slice marshaling** | Marshals as `null` | Marshals as `[]` | 
| **`nil` map marshaling** | Marshals as `null` | Marshals as `{}` | 
| **`omitempty` tag meaning** | Go zero value (ignores structs) | Empty JSON value ( `null` ,`""` ,`[]` ,`{}` ) | 
| **`time.Duration` default** | Integer nanoseconds ( `int64` ) | Reports error unless format specified | 
| **Stream reading (`io.Reader`)** | `NewDecoder(r).Decode(v)` (ignores trailing junk) | `json.UnmarshalRead(r, v)` (verifies full stream) | 
| **Custom streaming interface** | None ( `[]byte` buffer only) | `MarshalerTo` &`UnmarshalerFrom` | 
| **Caller-side type overrides** | Impossible (requires wrapper types) | `WithMarshalers` /`WithUnmarshalers` | 

## The Hidden $O(N^2)$ Performance Trap in `v1` Interfaces

Why couldnât the Go team just optimize `encoding/json` under the hood and keep
the `v1` API?

Because the public signatures of `json.Marshaler` and `json.Unmarshaler` in
`v1` mathematically force extra allocations and repeated parsing:

```
// The v1 interfaces commit the runtime to []byte allocations:
type Marshaler interface {
    MarshalJSON() ([]byte, error)
}
type Unmarshaler interface {
    UnmarshalJSON([]byte) error
}
```
Look closely at what happens when `v1` calls `MarshalJSON()`:

1. Your method allocates a fresh `[]byte` on the heap and returns it.
2. `encoding/json` cannot trust that your byte slice is valid JSON, so it must
parse the entire returned`[]byte`*again* to validate its syntax.
3. It then reformats and copies those bytes to match the outer indentation and escaping rules.

Now look at `UnmarshalJSON([]byte)`. It is even worse:

1. Before `encoding/json` can call your`UnmarshalJSON` method, it has to hand
you a complete, self-contained`[]byte` representing just that JSON value
with no trailing bytes.
2. To find where that JSON value ends, `encoding/json` must lex and parse the
entire JSON subtree from start to finish.
3. Then it slices those bytes and passes them to `UnmarshalJSON` , which
immediately calls`json.Unmarshal` to parse the exact same bytes a second
time.

What happens when you have a recursive data structure, like an OpenAPI schema
where a `Schema` struct contains nested `Schema` properties, and each level
implements `UnmarshalJSON`?

The outer node scans 100% of the JSON. The child node scans 90% of the JSON
again. The grandchild scans 80% again. **Parsing becomes quadratic ($O(N^2)$).**

This is not a theoretical benchmark contrived in a lab. It is the exact wall
Kubernetes hit when parsing recursive OpenAPI specifications in
`kubernetes/kube-openapi#315`<sup>2</sup>. Switching that recursive path from `v1`âs
byte-slice unmarshaling to `v2`âs streaming unmarshaler improved throughput by
orders of magnitude.

## The Architectural Split: `jsontext` vs `json/v2`

To break out of the $O(N^2)$ trap without breaking backwards compatibility, the Go team split JSON support into two packages.

### Layer 1: `encoding/json/jsontext` (Pure Syntax)

The new `encoding/json/jsontext` package takes its name directly from Section 2
of RFC 8259, where the formal grammar is called `JSON-text`. It knows how to
read and write JSON tokens and values over streams without touching Go
reflection:

```
package jsontext
type Encoder struct { ... }
func NewEncoder(io.Writer, ...Options) *Encoder
func (*Encoder) WriteValue(Value) error
func (*Encoder) WriteToken(Token) error
type Decoder struct { ... }
func NewDecoder(io.Reader, ...Options) *Decoder
func (*Decoder) ReadValue() (Value, error)
func (*Decoder) ReadToken() (Token, error)
type Kind byte
type Value []byte // json.RawMessage in v1 is now an alias for jsontext.Value
type Token struct { ... }
```
In `v1`, calling `Decoder.Token()` boxed strings and numbers into `any`
interface values, triggering heap allocations on every token. In `jsontext`,
`Token` is an opaque value type designed to represent arbitrary tokens **with
zero heap allocations**.

### Layer 2: `encoding/json/v2` (Semantics on Top of Syntax)

Once syntax lives in `jsontext`, the high-level `encoding/json/v2` surface
becomes remarkably symmetrical:

```
package json
func Marshal(in any, opts ...Options) (out []byte, err error)
func MarshalWrite(out io.Writer, in any, opts ...Options) error
func MarshalEncode(out *jsontext.Encoder, in any, opts ...Options) error
func Unmarshal(in []byte, out any, opts ...Options) error
func UnmarshalRead(in io.Reader, out any, opts ...Options) error
func UnmarshalDecode(in *jsontext.Decoder, out any, opts ...Options) error
```
Notice the progression in those three pairs:

- `Marshal` and`Unmarshal` work on`[]byte` in memory, just like`v1` , except
they now accept variadic`Options` directly.
- `MarshalWrite` and`UnmarshalRead` operate directly on`io.Writer` and`io.Reader` . Unlike`v1` âs`NewDecoder(r).Decode(v)` ,`UnmarshalRead` parses
the entire stream and rejects trailing junk at the end of the input.
- `MarshalEncode` and`UnmarshalDecode` operate directly on`*jsontext.Encoder` and`*jsontext.Decoder` . In fact, the first two pairs are simply thin
wrappers around`MarshalEncode` and`UnmarshalDecode` .

Six functions. Three levels of abstraction. One underlying streaming engine.

## Streaming Without Allocations: `MarshalerTo` and `UnmarshalerFrom`

With `jsontext.Encoder` and `jsontext.Decoder` in place, `v2` introduces two
new interfaces alongside the classic `Marshaler` and `Unmarshaler`:

```
type MarshalerTo interface {
    MarshalJSONTo(*jsontext.Encoder) error
}
type UnmarshalerFrom interface {
    UnmarshalJSONFrom(*jsontext.Decoder) error
}
```
Look at what disappears when a type implements `MarshalJSONTo` and
`UnmarshalJSONFrom`:

1. **Zero intermediate `[]byte` allocations:** Your method writes tokens or
values directly into the shared`*jsontext.Encoder` buffer.
2. **Zero double-parsing:** The decoder does not need to pre-scan the JSON to
find the closing brace before calling`UnmarshalJSONFrom` . Your method reads
directly from the live`*jsontext.Decoder` cursor in a single pass.
3. **Options propagate down the call stack:** In`v1` , if you set`Decoder.DisallowUnknownFields()` , that setting was lost the moment a nested
struct implemented`UnmarshalJSON([]byte)` . In`v2` , the`*jsontext.Decoder` carries the callerâs`Options` all the way down the recursion tree.

That single change is why `encoding/json/v2` unmarshals up to **10x faster**
than `v1` in `go-json-experiment/jsonbench`<sup>3</sup>, while keeping `Marshal`
at rough parity (with further `Marshal` optimizations slated for Go 1.28).

## First-Class Options and Caller-Side Overrides (`WithMarshalers`)

Every Go developer has eventually hit the âthird-party structâ problem. You
import a library struct (or a standard library type like `time.Duration`), and
you need to serialize it in a specific format, but you do not own the type so
you cannot attach a `MarshalJSON` method to it.

In `v1`, your only option was to write a boilerplate shadow struct or wrapper
type and copy fields back and forth.

In `v2`, the *caller* can inject custom marshalers and unmarshalers for any
type at the callsite using generic helper functions:

```
package main
import (
    "encoding/json/jsontext"
    "encoding/json/v2"
    "fmt"
    "time"
)
type JobConfig struct {
    Name    string        `json:"name"`
    Timeout time.Duration `json:"timeout"`
}
func main() {
    cfg := JobConfig{Name: "index-sync", Timeout: 45 * time.Second}
    // Override how time.Duration is marshaled across the entire call tree
    // without writing wrapper types or modifying JobConfig:
    durMarshaler := json.MarshalToFunc(
        func(enc *jsontext.Encoder, d time.Duration) error {
            return enc.WriteToken(jsontext.String(d.String()))
        },
    )
    out, err := json.Marshal(cfg, json.WithMarshalers(durMarshaler))
    if err != nil {
        panic(err)
    }
    fmt.Println(string(out))
    // Output: {"name":"index-sync","timeout":"45s"}
}
```
The Go blog highlights how this unlocks first-class Protocol Buffer
interoperability: a caller can register `proto.Message` via `WithMarshalers`
and let `protojson` handle every embedded protobuf message inside a standard Go
struct tree automatically.

## Why This Matters When You Build Local-First Go Tooling

I pay close attention to standard library ergonomics because of how I build my own tools.

Over the past year, as I wrote about in When the Hard Languages Stopped Being
Hard, I moved almost all
of my local-first engineering tools into pure Go with `CGO_ENABLED=0` and zero
third-party dependencies. In Thermal,
AgentMeter, and Sila, the hot path is
almost entirely JSON and JSONL.

When you are scanning tens of thousands of JSON-RPC messages from Model Context
Protocol servers, or replaying multi-gigabyte JSONL session transcripts across
fourteen different coding harnesses to audit why coding agents burn tokens in
the dark, every detail
in `encoding/json` shows up in your profiler:

- You cannot afford third-party C bindings or bloated reflection dependencies
when your binary needs to compile cleanly everywhere with `CGO_ENABLED=0` .
- You cannot afford `UnmarshalJSON([]byte)` allocating millions of short-lived
byte slices while walking nested tool-call envelopes inside a local MCP
stack.
- And you definitely cannot afford silent UTF-8 corruption or duplicate-key ambiguity when verifying cryptographic ledger receipts.

Having `encoding/json/jsontext` in the standard library means pure-Go CLI tools
get a zero-allocation, RFC-strict streaming token engine out of the box,
without pulling in external dependencies or hand-rolling brittle byte scanners.

## The Compatibility Trick: `v1` Is Now `v2` in a Trench Coat

Whenever a language introduces a `v2` of its most-imported package, the biggest
risk is ecosystem bifurcation. What happens if your application uses
`encoding/json/v2`, but a database driver you import still implements `v1`âs
`MarshalJSON`? Or what if your application stays on `encoding/json` (`v1`), but
a library you upgrade switches to `v2`âs `MarshalJSONTo` for performance?

In most ecosystems, one side breaks.

The Go team solved this with a remarkably clean architectural inversion: **in
Go 1.27, `encoding/json` (`v1`) is implemented entirely on top of
`encoding/json/v2`.**

Every single behavioral difference between `v1` and `v2`, whether duplicate key
handling, case-insensitive field lookup, or `nil` slice serialization, is
controlled by an explicit `Option` in `v2`. That allowed the Go team to rewrite
`encoding/json` (`v1`) as a thin wrapper that calls `encoding/json/v2` with
`jsonv1.DefaultOptionsV1()`.

Look at what that achieves immediately:

- **Bidirectional interface support:** If your`v1` code marshals a third-party
type that implements`v2` âs`MarshalJSONTo` ,`v1` calls`MarshalJSONTo` natively under the hood. If your`v2` code marshals a legacy type that only
implements`v1` âs`MarshalJSON` ,`v2` calls it seamlessly.
- **New tags work in `v1`:** Backwards-compatible struct tag additions from`v2` (such as`omitzero` or`inline` /`embed` improvements) work even before
you change your import path.
- **Granular, one-flag-at-a-time migration:** You do not have to flip an entire
monorepo from`v1` semantics to`v2` semantics in a single pull request.

### How to migrate gradually without breaking production

If you want strict `v2` behavior everywhere *except* that one legacy partner
webhook still sends duplicate keys, you opt out of just that rule:

```
out, err := json.Marshal(
    payload,
    json.DefaultOptionsV2(),
    jsontext.AllowDuplicateNames(true),
)
```
Or if you have a critical `v1` service and want to keep every legacy `v1`
quirk *except* that you want to close the duplicate-key security hole today:

```
out, err := json.Marshal(
    payload,
    jsonv1.DefaultOptionsV1(),
    jsontext.AllowDuplicateNames(false),
)
```
Two packages. One engine. Zero ecosystem split.

## The Larger Lesson

It is worth pausing on the closing paragraph of the Go teamâs announcement. The
six-year effort to design, prototype, benchmark, and ship `encoding/json/v2`
was largely driven and promoted by contributors *outside* Google, beginning with
Daniel MartÃ and Joe Tsai (who adapted the initial streaming prototype from the
Go Protocol Buffers serializer) alongside Roger Peppe, Chris Hines, Johan
Brandhorst-Satzkorn, Damien Neil, Austin Clements, and Michael Pratt.

Between Go 1.26âs NUMA-aware scheduler and Green Tea garbage collector, and Go
1.27âs `encoding/json/v2` and `jsontext`, the Go standard library is quietly
absorbing the performance and safety work that used to push teams toward
third-party replacement libraries.

From now on, when you write JSON code in Go 1.27+, ask yourself two questions:

1. **Am I mapping Go structs, or am I streaming JSON grammar?** Use`encoding/json/v2` (`MarshalWrite` /`UnmarshalRead` ) for structs, and drop
down to`encoding/json/jsontext` (`MarshalerTo` /`UnmarshalerFrom` ) the
moment custom serialization touches a hot path.
2. **Which `v1` defaults was my service accidentally relying on?** Test your
endpoints against case-sensitive field matching, non-null empty slices, and
duplicate-key rejection before a downstream parser discovers the difference
for you.

## Footnotes

1. 
Michael Pratt, Joe Tsai, Daniel MartÃ, Johan Brandhorst-Satzkorn, Roger Peppe, Chris Hines, and Damien Neil, âA new Go API for JSONâ, The Go Blog, November 4, 2026. â©
2. 
Kubernetes OpenAPI issue #315, documenting quadratic overhead in recursive `UnmarshalJSON` calls. â©
3. 
Go JSON Experiment Benchmarks, `go-json-experiment/jsonbench` . â©

Last updated on

FAQ

## Questions, answered

Also published as structured data for search.

## What is the difference between encoding/json/jsontext and encoding/json/v2 in Go 1.27?

The encoding/json/jsontext package handles pure RFC 8259 JSON syntax (streaming tokens and values with zero Go reflection), while encoding/json/v2 layers semantic mapping between JSON values and Go types on top of jsontext.

## Why is encoding/json/v2 up to 10x faster at unmarshaling than v1?

In v1, UnmarshalJSON required scanning an entire JSON value to find its boundary, allocating a byte slice, and then parsing that slice a second time inside the method. In v2, UnmarshalJSONFrom streams directly from a jsontext.Decoder without intermediate allocations or double parsing.

## Does encoding/json/v2 break existing Go 1 code using encoding/json?

No. Under the hood in Go 1.27, the original encoding/json package is implemented on top of encoding/json/v2 using v1 compatibility options (jsonv1.DefaultOptionsV1), allowing v1 and v2 packages to interoperate in the same binary.

Recommended

## Continue reading

Selected from shared topics, related tags, and the recent archive.
