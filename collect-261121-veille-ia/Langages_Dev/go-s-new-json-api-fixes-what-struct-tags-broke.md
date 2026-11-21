---
id: collect-261121-veille-ia/langages-dev/go-s-new-json-api-fixes-what-struct-tags-broke
title: "Go's New JSON API Fixes What struct Tags Broke"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "jsontext", "encoding", "field", "tags", "omitzero", "value", "meta"]
source: https://news.everbuild.cloud/gos-new-json-api-fixes-what-struct-tags-broke/
source_anchor: ""
source_lines: [1, 66]
section: "Langages_Dev"
---
Every Go codebase has that one file. Pointer-to-string fields, hand-written `MarshalJSON` wrappers, a `time.Time` that serializes as `null` no matter what tag you throw at it. The Go team just published a ground-up redesign of JSON handling on the official Go blog, built specifically to make that file unnecessary.

This isn't a patch on `encoding/json`. It's a new API with new semantics, and it's worth an hour of your attention today.

## Why this matters

`encoding/json` is one of the most-imported packages in the standard library, and its quirks cost real engineering hours: `omitempty` that ignores structs entirely, custom `UnmarshalJSON` methods that receive a fully-buffered `[]byte`, case-insensitive field matching that silently accepts sloppy payloads, and no clean way to forward raw JSON without a decode/re-encode round trip.

Most teams responded with workarounds — jsoniter, easyjson, reflection-free codegen. Those workarounds now have a standard-library answer.

## How it works

The redesign centers on two packages — `encoding/json/v2` and `encoding/jsontext` — and one idea: behavior belongs in options you pass per call, not in struct-tag archaeology.

**omitzero**
`omitempty` should have been. Omits any field holding its zero value, including structs and`time.Time` .
- **`jsontext.Value`** — raw JSON as a first-class type. Store it, forward it, never decode it.
- **Streaming custom types** —`MarshalJSONV2` and`UnmarshalJSONV2` hand you an encoder/decoder instead of a buffer, so custom marshaling stops materializing byte slices.
- **Sane defaults** — exact, case-sensitive field matching, plus options like`Deterministic` for stable map-key ordering and`RejectUnknownMembers` for strict ingestion.

The new implementation is also faster in the team's benchmarks. Fewer allocations, less reflection overhead.

## Where this helps

- **Gateways and proxies.** A`jsontext.Value` field carries an arbitrary payload section untouched — no schema drift, no 64-bit IDs mangled into floats, no key reordering.
- **PATCH-style DTOs.**`omitzero` finally distinguishes "field not set" from "field set to zero" without pointer gymnastics.
- **Cache keys and golden tests.**`Deterministic: true` sorts map keys, so serialized output becomes hashable and diffable.
- **Large ingestion.**`UnmarshalRead` parses straight from an`io.Reader` — stream a 500MB NDLOG file without buffering it first.

## Watch out

Availability depends on your toolchain. If `json/v2` isn't on by default for you yet, it's behind `GOEXPERIMENT=jsonv2` — check `go.dev/blog/jsonv2` and pin your toolchain before shipping it.

Semantics differ from v1 in both directions. Case-sensitive matching will drop fields from clients that relied on sloppy capitalization. `omitempty` doesn't behave identically either, so don't port tags blind — write a golden-file test first. And third-party libraries still speak the v1 interfaces; they'll keep working, but they won't get the streaming wins until they adopt v2.

## Try it yourself

```
# if json/v2 isn't default in your toolchain: GOEXPERIMENT=jsonv2 go run .
package main
import (
	json "encoding/json/v2"
	"encoding/jsontext"
	"fmt"
)
type Task struct {
	ID   int            `json:"id,omitzero"`
	Note string         `json:"note,omitzero"`
	Meta jsontext.Value `json:"meta"` // raw JSON, forwarded untouched
}
func main() {
	opts := json.MarshalOptions{Deterministic: true}
	b, err := opts.Marshal(Task{Meta: jsontext.Value(`{"priority":"high","tags":["urgent"]}`)})
	if err != nil {
		panic(err)
	}
	fmt.Println(string(b)) // {"meta":{"priority":"high","tags":["urgent"]}}
}
```
Zero `ID` and `Note` vanish. `Meta` passes through byte-for-byte. That's the whole pitch.

## TL;DR

- **What changed:** Go has a redesigned JSON API —`json/v2` plus`jsontext` — with per-call options,`omitzero` , and raw pass-through values.
- **Why it matters:** it retires the`omitempty` hacks, buffered custom marshalers, and decode/re-encode round trips polluting every Go API codebase.
- **Try today:** run the snippet, then port your ugliest DTO to`omitzero` +`jsontext.Value` and delete the wrapper file.
