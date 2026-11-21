---
id: collect-261121-veille-ia/langages-dev/go-1-27-encoding-json-v2-10x-faster-unmarshal-stricter-defaults
title: "Go 1.27 encoding/json/v2: 10x Faster Unmarshal, Stricter Defaults"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "error", "options", "behavior", "encoding", "jsontext", "func", "opts"]
source: https://www.neoteric.no/blog/go-1-27-encoding-json-v2-migration
source_anchor: ""
source_lines: [1, 81]
section: "Langages_Dev"
---
Go 1.27 makes `encoding/json/v2` and `encoding/json/jsontext` stable and generally available. The Go blog post (4 November 2026) lists the behavior changes, the performance claims and the migration mechanism. If you write Go services that touch JSON, read the behavior list before you flip anything.

## What v2 changes by default

The Go team says the goal is that v1 usage "will operate mostly the same if directly migrated to v2." Mostly. These defaults differ:

| Behavior | v1 | v2 | 
|---|---|---|
| Invalid UTF-8 | accepted | error | 
| Duplicate object member names | accepted | error | 
| nil slice / nil map | marshals as `null` | marshals as `[]` /`{}` | 
| Member name to struct field match | case-insensitive | case-sensitive | 
| `omitempty` | v1 semantics | omits fields that would encode as `null` ,`""` ,`[]` or`{}` | 
| `time.Duration` | has no default representation | error unless the caller picks one via options | 

The duplicate-name rejection matters for security. The post points out that duplicate names can be exploited in security applications and cites CVE-2017-12635. The case-insensitive match is described as "a surprising default, a potential security vulnerability, and a performance limitation."

The nil-slice change follows a survey cited in the post. It indicated most Go users prefer an empty array or object. That also avoids errors in other JSON implementations that refuse `null` where an array is expected.

## Performance: where the 10x comes from

The claims are specific:

- **Marshal:** roughly at parity with v1. Sometimes slightly faster, sometimes slightly slower.
- **Unmarshal:** "significantly faster," with benchmarks showing up to 10x.
- **Go 1.28:** further optimizations for cases where v2 is currently slower than v1.

The larger gains require code changes. Types that implement `MarshalJSON` or `UnmarshalJSON` should also implement the new streaming interfaces, `MarshalJSONTo` and `UnmarshalJSONFrom`. The v1 interfaces force allocation and double parsing. `UnmarshalJSON` receives a complete value, so the library parses it once to find its end and your method parses it again.

The post gives a real case. Recursive parsing of OpenAPI specifications in `UnmarshalJSON` methods hurt a Kubernetes service (kubernetes/kube-openapi#315). Switching to `UnmarshalJSONFrom` improved performance "by orders of magnitude." Benchmark details are in the go-json-experiment/jsonbench repository.

The catch: don't expect a 10x gain on a plain-struct service from a swap alone. The headline number is an upper bound, and Marshal-heavy code gets no speedup.

## The new API surface

The v2 entry points take options as a first-class argument:

```
func Marshal(in any, opts ...Options) (out []byte, err error)
func MarshalWrite(out io.Writer, in any, opts ...Options) error
func MarshalEncode(out *jsontext.Encoder, in any, opts ...Options) error
func Unmarshal(in []byte, out any, opts ...Options) error
func UnmarshalRead(in io.Reader, out any, opts ...Options) error
func UnmarshalDecode(in *jsontext.Decoder, out any, opts ...Options) error
```
`UnmarshalRead` addresses a v1 trap. `json.NewDecoder(r).Decode(v)` does not reject trailing junk after the value. Callers can also override per-type encoding with `WithMarshalers` and `MarshalFunc` / `MarshalToFunc`, and these take precedence over type-defined methods.

`jsontext` holds the syntactic layer: `Encoder`, `Decoder`, `Value` and `Token`. It does not depend on reflection. `RawMessage` in v1 is now a type alias to `jsontext.Value`.

## Migrating one behavior at a time

v1 is now implemented on top of v2 with the full set of v1 compatibility options. That lets you migrate per call site. Per the post, every behavior difference is controlled by an option, and encoding/json documents which option controls which behavior.

Take v2 behavior but still allow duplicate names:

```
json.Marshal(..., json.DefaultOptionsV2(), jsontext.AllowDuplicateName(true))
```
Or stay on v1 behavior but reject duplicates:

```
json.Marshal(..., jsonv1.DefaultOptionsV1(), jsontext.AllowDuplicateName(false))
```
The second form is where I would start on a production service. It adopts the safer duplicate-name behavior and nothing else changes.

## Do you have to migrate?

No. Both packages fall under the Go 1 compatibility promise. The post states that v1 applications "will continue to be supported indefinitely and need not ever migrate to v2."

Mixed codebases also work. A v2 caller handles a dependency's `MarshalJSON` method. A v1 caller handles a dependency's `MarshalJSONTo` method. A dependency adopting the streaming methods will not break you.

What I would do:

1. Move to Go 1.27 and read the v1/v2 comparison in the `encoding/json` docs.
2. Add `MarshalJSONTo` /`UnmarshalJSONFrom` to your hot custom types. Your JSON-heavy hot paths are the likeliest place to see the Unmarshal gain.
3. Check what your API clients send. Case-sensitive matching and duplicate-name rejection will surface as errors for sloppy clients.
4. Migrate call sites using the option combinations above, one behavior at a time.

The post says the effort took about six years, starting from a late-2020 draft by Daniel MartÃ and a prototype with Joe Tsai. It was a Go experiment in Go 1.25. A migration guide for `encoding/json/v2` and a "Working with JSON" tutorial are linked from the source.

Sources:
