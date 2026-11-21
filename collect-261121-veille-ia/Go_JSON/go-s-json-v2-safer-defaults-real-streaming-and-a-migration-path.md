---
id: collect-261121-veille-ia/go-json/go-s-json-v2-safer-defaults-real-streaming-and-a-migration-path
title: "Go's JSON v2: Safer Defaults, Real Streaming, and a Migration Path"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["json", "value", "defaults", "encoding", "package", "streaming", "data", "original"]
source: https://0xgosu.dev/blog/go-json-v2-streaming-migration/
source_anchor: ""
source_lines: [1, 50]
section: "Go_JSON"
---
# Go's JSON v2: Safer Defaults, Real Streaming, and a Migration Path

Go programs have had `encoding/json` since before Go 1.0. Its simple `Marshal` and `Unmarshal` functions made it easy to turn structs into wire data, but the package also fixed a set of behaviors that could not be changed without breaking existing programs. Go 1.27 introduces a stable `encoding/json/v2` beside the original package. The old import remains supported indefinitely; the new import offers stricter defaults, streaming primitives, and options at the call site.

The interesting part is the design boundary. JSON has a grammar, and an application has Go types. The new `encoding/json/jsontext` package handles the grammar. `encoding/json/v2` maps between JSON and Go values. Separating those jobs lets the runtime validate and stream tokens without making every custom type allocate a complete JSON value.

## Why the original API could not simply change

The original decoder accepts invalid UTF-8 in strings, replacing bad bytes, and it accepts duplicate object names. A duplicate such as `{"role":"user","role":"admin"}` has no single interpretation across JSON implementations: one consumer may keep the first value, another the last. That disagreement is dangerous when two services validate or authorize the same document. JSON v2 rejects both invalid UTF-8 and duplicate names by default.

Two other defaults change the shape of everyday payloads. A nil slice or map becomes `[]` or `{}` instead of `null`; object names match struct fields case sensitively instead of through the original loose case-insensitive matching. The `omitempty` tag now judges the JSON value that would be encoded, so it can omit `null`, an empty string, array, or object. Serializing `time.Duration` without choosing a representation now errors. These details can affect public APIs and stored data, so a package switch deserves contract tests.

Changing the old packageâs defaults would violate Goâs compatibility promise. The new package gives the project a separate namespace. Underneath, the original API now uses the v2 implementation with compatibility options, letting both versions share improvements while preserving the old behavior.

That split took years to reach a stable release. Maintainers explored the design from 2020, published a formal proposal in 2025, and shipped an experiment in Go 1.25. Production use and community feedback shaped the final API. The result is deliberately familiar at the top level while allowing much more control below it.

## A syntax engine that can stream

`jsontext.Decoder` reads a sequence of `Token` or complete `Value` units from an `io.Reader`; `jsontext.Encoder` writes them to an `io.Writer`. A token is a structural delimiter, name, or scalar. A value is an entire JSON value represented as bytes. The encoder and decoder maintain grammar state, so a stream of tokens still has to form valid JSON.

The high-level package keeps the familiar calls, now with variadic options:

```
import json "encoding/json/v2"
data, err := json.Marshal(record)
err = json.Unmarshal(data, &record)
```
It also has `MarshalWrite` and `UnmarshalRead` for writers and readers, plus `MarshalEncode` and `UnmarshalDecode` when a caller already has a `jsontext` encoder or decoder. This matters for an HTTP handler: it can connect parsing to a request body or encoding to a response stream without first constructing an intermediate byte slice merely to satisfy the API.

Reading from a stream still requires attention to document boundaries. A single decoded JSON value does not automatically mean the input had no trailing data. The new reader-oriented API gives callers a clearer whole-document operation; token-oriented code should decide explicitly whether another token or value is permitted. This distinction matters for request validation and signed payloads.

The old `MarshalJSON() ([]byte, error)` forces a custom marshaler to return a complete allocation. The old `UnmarshalJSON([]byte)` requires the enclosing decoder to find a whole value before handing it off, after which the method often parses that value again. JSON v2 still understands those interfaces, but adds `MarshalJSONTo(*jsontext.Encoder)` and `UnmarshalJSONFrom(*jsontext.Decoder)`. A type can write or read directly through the streaming syntax layer, including nested values, and inherit the callerâs options.

## Custom behavior belongs to both types and callers

Some types own their wire format, so type-defined methods remain useful. Other decisions belong to the caller. A service may need a particular encoding for a third-party type it cannot modify. JSON v2 supports caller-specified marshal and unmarshal functions through `WithMarshalers` and `WithUnmarshalers`; these take precedence over a typeâs own method and the default representation. That makes local policy possible without wrapping every value in a new Go type.

Options also reach further than before. In v1, `Encoder` and `Decoder` carried some settings while `Marshal` and `Unmarshal` did not. A custom `UnmarshalJSON` could obscure settings such as unknown-field rejection. In v2, the main functions accept options directly, and streaming custom methods can inspect the encoder or decoder options. The same policy can follow a value through nested calls.

## Performance without a blanket speed claim

The Go team reports `Marshal` performance roughly on par with v1: some cases improve and others regress. `Unmarshal` is substantially faster in its benchmarks, reaching up to 10Ã in favorable cases. That figure is a benchmark ceiling, not a promise for every service. The largest gains for custom types require adopting the streaming interfaces, especially when recursive old-style methods repeatedly parse or reformat whole values. Benchmark your own payloads and keep allocations and latency separate in the results.

## Migrate one behavior at a time

Simple call sites can often change the import and continue compiling. The difficult work is finding dependencies on old semantics. Test duplicate-name handling, malformed UTF-8, nil collections, field-name case, `omitempty`, duration fields, custom marshaler methods, and readers with trailing input. Include golden wire payloads and interoperability tests where another service consumes the JSON.

For a cautious migration, first call v2 with `jsonv1.DefaultOptionsV1()` so the import changes while behavior stays familiar. Then override individual options as contracts are reviewed. The Go migration guide also describes `jsonsplit`, a wrapper that can run both paths in production, report differences, and still return the v1 result until a service is ready to switch. Comparing both paths costs extra CPU, so use it as a migration tool rather than a permanent default.

Neither application dependencies nor shared libraries need to move in lockstep. V2 recognizes the original `MarshalJSON` interfaces, and v1 recognizes the new streaming methods through its v2-based implementation. The old package remains available. That compatibility makes adoption a measured engineering choice: start where stricter parsing or streaming solves a concrete problem, then expand with evidence from tests and real traffic.
