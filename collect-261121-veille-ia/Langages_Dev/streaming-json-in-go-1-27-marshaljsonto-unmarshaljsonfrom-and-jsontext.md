---
id: collect-261121-veille-ia/langages-dev/streaming-json-in-go-1-27-marshaljsonto-unmarshaljsonfrom-and-jsontext
title: "Streaming JSON in Go 1.27: MarshalJSONTo, UnmarshalJSONFrom, and jsontext"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "jsontext", "error", "type", "encoder", "func", "unmarshaljsonfrom", "value"]
source: https://www.bytesizego.com/blog/streaming-json-in-go-1-27-marshaljsonto-unmarshaljsonfrom-an
source_anchor: ""
source_lines: [1, 178]
section: "Langages_Dev"
---
# Streaming JSON in Go 1.27: MarshalJSONTo, UnmarshalJSONFrom, and jsontext

Most of the Go 1.27 JSON coverage is about behavior: invalid UTF-8 now errors, duplicate object names now error, and field matching is case-sensitive. Each of those changes has a compatibility option, so you can retain the old semantics where your code needs them.

The part that requires you to write code is the pair of new interfaces: `MarshalerTo` and `UnmarshalerFrom`. `encoding/json/v2` and `encoding/json/jsontext` are stable and generally available in Go 1.27, per the official Go blog announcement, and if you maintain a type with a custom `MarshalJSON` or `UnmarshalJSON` method, these are the methods worth adding. The Go blog reports that recursive OpenAPI parsing in `UnmarshalJSON` methods was hurting a particular Kubernetes service badly, and switching to `UnmarshalJSONFrom` improved performance by orders of magnitude (kubernetes/kube-openapi#315).

## Why the old interfaces cap your performance

The limitation is in the signatures, not just the implementation behind them.

```
type Marshaler interface {
    MarshalJSON() ([]byte, error)
}
```
Returning `[]byte` forces an allocation on every call. And the `json` package canât trust what comes back, so it has to verify the bytes are valid JSON and reformat them to match the requested indentation.

Unmarshaling has the mirror-image flaw:

```
type Unmarshaler interface {
    UnmarshalJSON([]byte) error
}
```
The method requires a complete JSON value with no trailing data. So the package parses the value in full just to locate where it ends, hands you the bytes, and then your method parses those same bytes again. Two passes over the same input, every time.

It gets worse. If your `UnmarshalJSON` recursively calls `json.Unmarshal`, which is the obvious way to write a wrapper type, performance becomes quadratic. Thatâs the trap the Kubernetes OpenAPI code fell into.

## The syntax/semantics split

Fixing this meant separating two concerns that `encoding/json` had merged. The Go blog is careful with vocabulary here: *encode* and *decode* refer to syntactic work, processing JSON according to its grammar, while *marshal* and *unmarshal* refer to semantic work, the relationship between JSON values and Go values.

`jsontext` owns the syntax half. The name comes from RFC 8259, section 2, where the grammar for JSON data is literally called `JSON-text`. Because it works only at the syntactic level, it has no dependency on reflection:

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
```
`jsontext.Value` is a named `[]byte`. Itâs identical to v1âs `RawMessage`, which is now a type alias to it. `jsontext.Token` is an opaque struct with constructors and accessor methods, designed to represent arbitrary JSON tokens without allocating. The v1 `Token` was allocation-heavy, and there was no token-writing API at all.

The other difference: unlike the v1 `Encoder` and `Decoder`, which buffer the entire JSON value in memory despite taking an `io.Writer` or `io.Reader`, these stream for real.

## Writing MarshalJSONTo and UnmarshalJSONFrom

The new interfaces take those streaming types:

```
type MarshalerTo interface {
    MarshalJSONTo(*jsontext.Encoder) error
}
type UnmarshalerFrom interface {
    UnmarshalJSONFrom(*jsontext.Decoder) error
}
```
Now the `json` package isnât responsible for validating or formatting your output. The `Encoder` is. And it isnât responsible for finding the boundaries of your value before calling you. The `Decoder` is.

v2 gives you functions that operate directly on those types, which makes delegation straightforward:

```
func MarshalEncode(out *jsontext.Encoder, in any, opts ...Options) error
func UnmarshalDecode(in *jsontext.Decoder, out any, opts ...Options) error
```
Hereâs an illustrative example built from documented APIs, not code lifted from a real project. Say you store temperature internally in Kelvin but serialize Celsius:

```
package main
import (
    "encoding/json/jsontext"
    json "encoding/json/v2"
)
type Temperature struct {
    Kelvin float64
}
func (t Temperature) MarshalJSONTo(enc *jsontext.Encoder) error {
    return json.MarshalEncode(enc, t.Kelvin-273.15)
}
func (t *Temperature) UnmarshalJSONFrom(dec *jsontext.Decoder) error {
    var celsius float64
    if err := json.UnmarshalDecode(dec, &celsius); err != nil {
        return err
    }
    t.Kelvin = celsius + 273.15
    return nil
}
```
Put that next to the v1 shape. `MarshalJSON` has to build a `[]byte`, often through a nested `json.Marshal` call that allocates, hand it back, and let the package re-scan it. The v2 version writes directly to the encoderâs stream.

You donât have to delete your existing methods to do this. v2 supports `Marshaler` and `Unmarshaler` unchanged from v1, so adding the streaming methods is purely additive. v2 prefers them when present, and v1 callers get the benefit too, because v1 is now implemented in terms of v2 and picks up `MarshalJSONTo` under the hood.

## Options now flow down the call stack

This is the quieter improvement, and it fixes something that has irritated people for years. In v1, `Decoder.DisallowUnknownFields` lost its effect the moment a custom `UnmarshalJSON` method took over. The method had no idea what options the caller set and no way to forward them.

In v2, options are a first-class argument to every entry point:

```
func Marshal(in any, opts ...Options) (out []byte, err error)
func MarshalWrite(out io.Writer, in any, opts ...Options) error
func MarshalEncode(out *jsontext.Encoder, in any, opts ...Options) error
func Unmarshal(in []byte, out any, opts ...Options) error
func UnmarshalRead(in io.Reader, out any, opts ...Options) error
func UnmarshalDecode(in *jsontext.Decoder, out any, opts ...Options) error
```
Your `MarshalJSONTo` receives the `*jsontext.Encoder`, and the `Encoder` exposes an `Options` accessor method, so you can read the callerâs configuration from inside your own method. Delegate with `json.MarshalEncode(enc, ...)` and the options ride along.

One more thing about those signatures. `UnmarshalRead` is what you want when reading from an `io.Reader`. The familiar `json.NewDecoder(r).Decode(v)` fails to reject trailing junk after the value, which has let malformed input slide through more codebases than anyone wants to count. If youâve been using the standard JSON-to-struct decoding patterns, this is a one-line fix.

## Customizing types you donât own

Often the type that needs special treatment belongs to someone else. v2 handles that with generic constructors:

```
func MarshalFunc[T any](fn func(T) ([]byte, error)) *Marshalers
func MarshalToFunc[T any](fn func(*jsontext.Encoder, T) error) *Marshalers
func WithMarshalers(*Marshalers) Options
func UnmarshalFunc[T any](fn func([]byte, T) error) *Unmarshalers
func UnmarshalFromFunc[T any](fn func(*jsontext.Decoder, T) error) *Unmarshalers
func WithUnmarshalers(*Unmarshalers) Options
```
This is a genuinely good use of type parameters: the type argument tells v2 which Go type your function applies to. Caller-specified functions take precedence over type-defined methods and over the default representation.

Which brings us to `time.Duration`. In v2, serializing a `time.Duration` reports an error, because it has no default representation and v2 decided to make the caller choose rather than guess. So choose:

```
package main
import (
    "encoding/json/jsontext"
    json "encoding/json/v2"
    "fmt"
    "time"
)
type Job struct {
    Name    string        `json:"name"`
    Timeout time.Duration `json:"timeout"`
}
func main() {
    durationAsString := json.WithMarshalers(
        json.MarshalToFunc(func(enc *jsontext.Encoder, d time.Duration) error {
            return json.MarshalEncode(enc, d.String())
        }),
    )
    out, err := json.Marshal(Job{Name: "reindex", Timeout: 90 * time.Second}, durationAsString)
    if err != nil {
        panic(err)
    }
    fmt.Println(string(out)) // {"name":"reindex","timeout":"1m30s"}
}
```
A `time.Duration` serialized as a bare integer of nanoseconds is a decision worth making deliberately rather than inheriting. The Go blogâs `ProtoJSON` example uses the same mechanism to route every `proto.Message` through `protojson`: one option value can customize an entire type hierarchy.

## Migrating one behavior at a time

Because v1 is implemented as v2 plus the full set of v1 compatibility options, migration isnât a cliff. Each behavior difference is controlled by one option, so you can move to v2 and keep the single v1 behavior your code actually depends on:

```
// All v2 behavior, except duplicate object names are still allowed.
json.Marshal(v, json.DefaultOptionsV2(), jsontext.AllowDuplicateNames(true))
```
Or go the other direction, staying on v1 semantics while tightening one thing:

```
// All v1 behavior, except duplicate names now produce an error.
json.Marshal(v, jsonv1.DefaultOptionsV1(), jsontext.AllowDuplicateNames(false))
```
The `encoding/json` package documentation lists every behavior difference alongside the option that controls it. That table is worth reading before you flip an import path.

## Where the gains actually show up

The Go blogâs numbers: `Marshal` in v2 is roughly at parity with v1, sometimes slightly faster and sometimes slightly slower. `Unmarshal` is significantly faster, with benchmarks showing improvements up to 10x. Details are in the go-json-experiment/jsonbench repository, and the team says more optimization work is coming in Go 1.28, specifically for cases where v2 is still slower than v1.

That asymmetry tells you where to spend your time. Moving call sites to v2 can deliver decoding gains, but it also opts into v2âs behavior changes, so use the compatibility options and migration guide where needed. The larger gains in custom types come from implementing `MarshalJSONTo` and `UnmarshalJSONFrom`, which avoids materializing and rescanning each custom JSON value as a separate `[]byte`. Audit recursive `MarshalJSON` and `UnmarshalJSON` implementations first; that is where quadratic behavior can hide. If youâre sizing up the rest of the release, the Go 1.27 changes roundup has the wider view.
