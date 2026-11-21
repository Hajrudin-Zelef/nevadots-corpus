---
id: collect-261121-veille-ia/langages-dev/go-json-v2-vs-v1-go-1-27-speed-and-silent-changes
title: "Go JSON v2 vs v1: Go 1.27 speed and silent changes"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "same", "encoding", "structs", "implementation", "marshal", "unmarshal", "speed"]
source: https://blogs.abhipanseriya.dev/blog/go-json-v2-vs-v1-go-1-27
source_anchor: ""
source_lines: [1, 135]
section: "Langages_Dev"
---
On November 4 the Go team published A new Go API for JSON, announcing that `encoding/json/v2` and `encoding/json/jsontext` are stable and generally available in Go 1.27, with Unmarshal benchmarks showing "improvements of up to 10x" over v1. If you are reading that as a reason to rewrite your imports for speed, check what your current build already does. Go 1.27 runs the old `encoding/json` API on the v2 engine, so most of the speedup arrived the day you bumped the toolchain.

Decoding 500 GitHub-issue-shaped structs on our machine took 3.63 ms with plain v1 on Go 1.24.13, 1.79 ms with the same unchanged code on Go 1.27.3, and 1.43 ms after switching to the v2 API. The last 20% has a specific cause, and it changes what your structs look like when input is broken. The real reason to migrate is v2's stricter defaults, and the package docs list sixteen behavior differences where the blog post highlights six.

## Go 1.27 already put the v2 engine under encoding/json

The Go 1.27 release notes say it in one line: "The encoding/json package is now backed by the v2 implementation." The v1 docs are more precise. `json.Marshal` and `json.Unmarshal` are "semantically identical" to calling `jsonv2.Marshal(v, jsonv1.DefaultOptionsV1())`, where that option set flips 21 switches back to legacy behavior. Same parser, same reflection code, different defaults. It is the move Prisma 7 made when it replaced its query engine this year: the speedup came from the machinery underneath, not from anything callers wrote.

To separate the toolchain from the engine, we ran one benchmark four ways on an Apple M3 Pro: a 635 KB payload of 500 structs with a nested user, label and assignee slices, two `time.Time` fields and two small maps, measured with `go test -run x -bench . -benchmem -count 6` (four runs for the `nojsonv2` row). Times are approximate medians.

| Build | Unmarshal | Allocs/op | Marshal | Allocs/op | 
|---|---|---|---|---|
| Go 1.24.13, v1 API | 3.63 ms | 16,518 | 0.90 ms | 7,004 | 
| Go 1.27.3, v1 API, `GOEXPERIMENT=nojsonv2` | 3.82 ms | 16,518 | 0.88 ms | 7,004 | 
| Go 1.27.3, v1 API (default) | 1.79 ms | 7,121 | 0.95 ms | 4,505 | 
| Go 1.27.3, v2 API | 1.43 ms | 7,121 | 0.91 ms | 2,004 | 

The `nojsonv2` row is the control. Same 1.27.3 compiler, old implementation, and Unmarshal goes straight back to 3.8 ms with the same 16,518 allocations as Go 1.24. The halving comes from the new decoder. Calling v2 with `DefaultOptionsV1()` landed at 1.79 ms too, which is what the docs promise: the v1 entry point is just v2 wearing the legacy options.

"Up to 10x" traces to the jsonbench repository, which compares against the old v1 implementation and reports v2 as "2.7x to 10.2x faster" for concrete types and "10.2x to 21.1x faster" for its RawValue types. Its explanation is that v1 "performs a virtual function call for every byte of input." The other source of big numbers is custom methods. The Go blog's example is Kubernetes, where "recursive parsing of OpenAPI specifications in `UnmarshalJSON` methods significantly hurt performance" and "switching to `UnmarshalJSONFrom` improved performance by orders of magnitude." Our payload has no custom unmarshalers, and the total there is about 2.5x from Go 1.24 to v2. Like TypeScript 7's 10x headline, the number is real for the workload it was measured on.

## Marshal got slower for typed structs, and the issue is milestoned for Go 1.28

The release notes call Marshal "broadly at parity with the previous implementation." Practitioners measured otherwise. Daniel Lemire benchmarked Go 1.27.0 on August 29 and concluded that marshalling "is about 1.5x slower for typed structs in my tests." Issue golang/go#81315 reproduced it: marshaling 10,000 small records went from 1.633 ms on the legacy implementation to 2.477 ms on Go 1.27, a 51.73% regression on an M4 Max. The same reporter measured a 102% regression on a Linux VM.

Our payload regressed less, 0.88 ms to 0.95 ms with the v1 API, about 8%, while allocations dropped from 7,004 to 4,505. The issue title puts the hit on slices of typed structs at 1.5x to 2x, so how much you lose depends on your payload. A contributor posted a stack of CLs that recovered all of it in their benchmark. The issue is still open, milestoned for Go 1.28.

If a marshal-heavy service regresses after the upgrade, the release notes give you an escape hatch: build with `GOEXPERIMENT=nojsonv2` to restore "the original v1 implementation." The same paragraph says "this opt-out is expected to be removed in a future release," so treat it as a temporary bridge. This is the command behind the control row above:

`GOTOOLCHAIN=local GOEXPERIMENT=nojsonv2 go test -run x -bench . -benchmem -count 4`
The release notes also warn that "the exact text of error messages may differ." We checked four common decode errors (truncated input, a string where an `int` belongs, a number where a `string` belongs, a trailing comma) on Go 1.24.13, Go 1.27.3 and `nojsonv2`. All three printed identical strings, starting with `unexpected end of JSON input`. We did not test rarer errors.

## The extra 20% from v2 is a validation pass you stop paying for

Going from 1.79 ms to 1.43 ms surprised us, because every behavior option sounds cheap. So we turned them on one at a time against the v2 call. Ten of them, including case-insensitive matching, duplicate-name tolerance, invalid UTF-8 tolerance and legacy merge, each stayed at roughly 1.4 ms, within noise of plain v2. One option, `ReportErrorsWithLegacySemantics`, pushed it to 1.94-2.00 ms on its own.

Its documentation explains why. With legacy semantics, "the syntactic structure of the JSON input is fully validated before performing the semantic unmarshaling." v1 reads your input twice: once to check it, once to decode it. v2 does "a streaming decode," which "means that the Go value may be partially mutated when a syntactic error is encountered." Both behaviors on a truncated request body:

```
type Account struct {
	ID      string `json:"id"`
	Plan    string `json:"plan"`
	Credits int    `json:"credits"`
}
truncated := []byte(`{"id":"acct_9","plan":"enterprise","credits":5`)
a1 := Account{ID: "old", Plan: "free"}
err1 := jsonv1.Unmarshal(truncated, &a1) // encoding/json
a2 := Account{ID: "old", Plan: "free"}
err2 := json.Unmarshal(truncated, &a2) // encoding/json/v2
```
```
v1: {ID:old Plan:free Credits:0} err=unexpected end of JSON input
v2: {ID:acct_9 Plan:enterprise Credits:5} err=jsontext: unexpected EOF after offset 46
```
With v1, a truncated body leaves your struct untouched. With v2, the account now says `enterprise` and the error is easy to log and move past. Duplicate keys behave the same way: v2 returned the error with `user_id` already filled in. Any code that decodes into a pre-populated value is exposed: config loaded over defaults, a cache entry refreshed in place, a PATCH handler decoding onto the stored record. Decode into a fresh zero value and copy it over only when `err == nil`.

## The blog lists six changes. The package docs list sixteen.

The Go blog's list covers invalid UTF-8, duplicate names, nil slices and maps, case-sensitive matching, `omitempty` and `time.Duration`. The "Migrating to v2" section of the encoding/json package docs has sixteen bullets, each naming the option that restores v1. The useful way to read them is by how they fail. Some return an error, and your tests catch those on the first run. Others return `nil` and different bytes, and those reach production.

| Change | v1 | v2 | How it fails | Option to keep v1 | 
|---|---|---|---|---|
| Field name matching | Case-insensitive | Exact | Silent: field stays zero | `MatchCaseInsensitiveNames` or the`case:ignore` tag | 
| `omitempty` on a number or bool | Omits 0 and false | Omits only null, "", {}, [] | Silent: `"retries":0` appears | `OmitEmptyWithLegacySemantics` , or switch to`omitzero` | 
| nil slice or map | `null` | `[]` or`{}` | Silent | `FormatNilSliceAsNull` ,`FormatNilMapAsNull` | 
| Map key order | Deterministic | Non-deterministic | Silent: hashes and snapshots change | `json.Deterministic(true)` | 
| HTML characters in strings | Escaped as `\u003c` | Written raw | Silent | `jsontext.EscapeForHTML` | 
| Byte array ( `[4]byte` ) | Array of numbers | Base64 string | Silent | `FormatByteArrayAsArray` | 
| JSON null into a set value | Zeroes it or does nothing | Always zeroes | Silent | `MergeWithLegacySemantics` | 
| `time.Duration` | Integer nanoseconds | No default | Error | `FormatDurationAsNano` | 
| Duplicate object names | Last one wins | Rejected | Error, value partly filled | `jsontext.AllowDuplicateNames` | 
| Invalid UTF-8 | Replaced with U+FFFD | Rejected | Error | `jsontext.AllowInvalidUTF8` | 
| Go array length mismatch | Any length accepted | Must match | Error | `UnmarshalArrayFromAnyLength` | 
| Malformed struct tags | Ignored | Runtime error | Error | `ReportErrorsWithLegacySemantics` | 

Four more (the narrower `string` tag option, pointer-receiver `MarshalJSON` being called on unaddressable values, methods on map keys, merge rules for slices) matter mostly to code with custom marshalers or code that decodes into populated slices. Here is what the silent ones looked like when we ran them on Go 1.27.3, v1 then v2, trimmed to the relevant fields:

```
v1 Marshal   {"id":"ord_1","items":null,"meta":null,"checksum":[1,2,3,4]}
v2 Marshal   {"id":"ord_1","items":[],"meta":{},"retries":0,"checksum":"AQIDBA=="}
v1 Unmarshal {"USER_ID":"u_42","admin":false} -> {UserID:u_42 Admin:false} err=<nil>
v2 Unmarshal {"USER_ID":"u_42","admin":false} -> {UserID: Admin:false} err=<nil>
v1 map order 1 distinct outputs in 1000 runs
v2 map order 8 distinct outputs in 1000 runs
```
Every change in that output came back with a `nil` error. The `retries` field had `omitempty` on an `int`, and v2 now sends `0`, because zero is a JSON number, not an empty value. The docs say to move those tags to `omitzero`, which works identically in both versions, and you can do that today without touching anything else. The case-mismatched key did not fail. It vanished, and anything downstream sees an empty user ID. The map ordering will break golden-file tests and any cache key, ETag or signature computed over marshaled bytes. Turning `Deterministic` back on cost us about 7% on Marshal and brought allocations from 2,004 back to 4,505, which suggests most of v1's Marshal allocations went to sorting keys.

HTML escaping is the one to think about rather than just restore. v1 escaped `<`, `>` and `&` so JSON could be dropped into a script tag without breaking out of it. v2 writes them raw. If you render JSON into HTML anywhere, keep `jsontext.EscapeForHTML(true)` on that path. Marshaling the same map on Go 1.27.3:

```
v1 {"q":"\u003cscript\u003ealert(1)\u003c/script\u003e \u0026 co"}
v2 {"q":"<script>alert(1)</script> & co"}
```
The loud changes are mostly improvements. Duplicate names are a known parser-differential trick (the Go blog links CVE-2017-12635), and rejecting them is the correct default for anything security-relevant.

## If you ran the jsonv2 experiment on Go 1.25 or 1.26, check your tags first

v2 spent two releases behind a GOEXPERIMENT, and some teams adopted its extras early. The 1.27 release notes list what changed before it went stable: the `format` tag option, the `unknown` tag option, `DiscardUnknownMembers` and `SkipFunc` were removed, and `inline` was renamed to `embed`. A removed tag breaks plain `encoding/json` calls too, because v1 is v2 underneath. Right after upgrading, one team filed golang/go#80962 with this error from a plain v1 `json.Marshal` call:

``json: unsupported value: Go struct field ParentAccountIds has unsupported `format` tag option``
They had the experiment enabled on 1.26. The maintainer reply was short: "We removed format for 1.27." An engine swap behind an unchanged API, like Bun's Rust rewrite, gets judged by the cases its old tests never covered, and experiment-only tags are that kind of case. Before upgrading, grep your structs for `format:`, `unknown` and `inline` inside json tags.

## Migrate by diffing production traffic

The official migration guide opens with "you don't have to!" and means it. v1 is covered by the compatibility promise, and the Go blog says v1 users "need not ever migrate." If you do migrate, it describes three paths. All at once means updating call sites, making sure the tests pass, and checking it in. Option by option starts by moving every call to v2 with `DefaultOptionsV1`, which the guide calls "a trivial and safe change," then peels off one legacy behavior at a time. The third, for production servers, is a dual-run library, which is shadow testing applied to a JSON codec. Later options override earlier ones, so the second path looks like this:

`b, err := json.Marshal(v, jsonv1.DefaultOptionsV1(), json.FormatNilSliceAsNull(false))`
For services, the stronger move is jsonsplit. It calls both versions, returns v1's bytes, and reports every difference along with the options that explain it. This is the setup from the migration guide:

```
jsonsplit.GlobalCodec.SetMarshalCallMode(jsonsplit.CallBothButReturnV1)
jsonsplit.GlobalCodec.AutoDetectOptions = true
jsonsplit.GlobalCodec.ReportDifference = func(d jsonsplit.Difference) {
	fmt.Printf("detected jsonv1-to-jsonv2 difference: %v\n", d)
}
```
We pointed it at an invoice struct with a nil slice, a nil map, a zero `omitempty` integer and an ampersand in a memo. One call produced this report, trimmed to the option list:

`"Options":["jsontext.EscapeForHTML","jsonv1.OmitEmptyWithLegacySemantics","jsonv2.FormatNilMapAsNull","jsonv2.FormatNilSliceAsNull"]`
That is the migration list for this type. The guide warns that running both versions "will approximately double the cost of marshaling," so sample with `SetMarshalCallRatio` rather than diffing every request.

### Bump jsonsplit's v2 dependency before you trust it

The current jsonsplit version (a pseudo-version dated August 14, 2026) still imports `github.com/go-json-experiment/json` pinned to a July 2025 snapshot for its v2 side, with a `// TODO: Use "encoding/json/v2"` comment. In our run, the four options it reported matched differences we saw from stdlib v2 directly. Left at that pin, it compares against the 2025 prototype rather than the real encoding/json/v2, so add `github.com/go-json-experiment/json@latest` alongside it; on Go 1.27 that module aliases the standard library. Our step-by-step migration guide covers the bump and what broke without it.

One papercut from the Hacker News thread is real: v2 has no `MarshalIndent`. Pretty-printing is an option now, and it combines with the others:

`b, err := json.Marshal(report, jsontext.WithIndent("  "), json.Deterministic(true))`
## What to do with your services this week

If you are still on Go 1.26 or older, upgrade. Unmarshal-heavy code (API gateways, consumers reading queues, anything parsing webhooks) gets faster with zero code changes. Before rollout, benchmark your hottest Marshal path with real structs. #81315 is still open as of Go 1.27.3, milestoned for Go 1.28. If it regresses badly, ship with `GOEXPERIMENT=nojsonv2`, re-run the benchmark on each release, and drop the flag once the regression is gone.

For new code, start on v2 directly. Use `omitzero` instead of `omitempty` on numbers and bools, set `Deterministic(true)` wherever bytes get hashed or signed, and decode into fresh values. For existing services with external consumers, the nil-slice and case-matching changes are contract changes, whatever the import path says. Move to v2 with `DefaultOptionsV1`, sample jsonsplit in production for a week, and flip options one at a time as the reports go quiet.

The thing to watch is #81315 and the CLs attached to it. If they land for Go 1.28 as milestoned, the remaining performance argument for staying on the legacy implementation goes away, and so, eventually, does `nojsonv2`.
