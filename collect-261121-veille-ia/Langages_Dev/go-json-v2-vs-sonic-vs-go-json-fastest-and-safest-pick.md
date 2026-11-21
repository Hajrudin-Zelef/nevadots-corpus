---
id: collect-261121-veille-ia/langages-dev/go-json-v2-vs-sonic-vs-go-json-fastest-and-safest-pick
title: "Go JSON v2 vs sonic vs go-json: Fastest and Safest Pick"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "go-json", "encoding", "sonic", "amd64", "arm64", "about", "bytedance"]
source: https://blogs.abhipanseriya.dev/compare/golang-json-v2-vs-sonic-vs-go-json
source_anchor: ""
source_lines: [1, 141]
section: "Langages_Dev"
---
On our Apple M3 Pro, ByteDance's bytedance/sonic encoded a 92 KB payload at 0.88x to 1.01x the speed of plain `encoding/json`, while goccy/go-json encoded it 1.7x to 3.2x faster. That gap is where Go JSON v2 vs sonic vs go-json gets decided. `encoding/json/v2` is the standard library's new API, stable since Go 1.27. sonic is built on a runtime JIT and SIMD assembly. go-json is a dependency-free drop-in that its author rewrote in September and October 2026.

Three facts settle the pick: the CPU you deploy to, whether you need v2's strict input checks, and how much maintenance risk a 2x to 4x gain is worth on a code path that may not show up in your profile at all.

## At a glance

Versions: Go 1.27.3, sonic v1.15.5, go-json v0.12.0, all checked on 2026-11-07. Speed figures are geometric means over nine payloads from go-json's own benchmark report (run 2026-11-05) unless marked as our run.

| Dimension | encoding/json/v2 | sonic | go-json | Winner | 
|---|---|---|---|---|
| Decode speed vs encoding/json | 1.0x (v2 API about 1.2x the v1 API in our run) | 2.2x amd64, 2.0x arm64 (ConfigStd) | 3.9x amd64, 4.0x arm64 | go-json | 
| Encode speed vs encoding/json | about 1.0x | 2.8x amd64, 1.06x arm64 | 3.5x amd64, 3.2x arm64 | go-json | 
| Rejects duplicate keys and invalid UTF-8 | Yes, by default | No, in every config tested | Only through go-json/v2 | encoding/json/v2 | 
| CPU and Go version support | Every Go platform | amd64 and arm64, Go 1.18 to 1.27 (not 1.24.0); falls back to encoding/json elsewhere | Any platform | encoding/json/v2 | 
| Extra modules pulled in | 0 | 7 | 0 | encoding/json/v2, go-json | 
| Binary size added (linux/amd64, our build) | 0 | about 3.9 MB | about 1.8 MB | encoding/json/v2 | 
| Who maintains it | Go team | ByteDance, several contributors | One maintainer, pre-1.0 | encoding/json/v2 | 
| Switching cost from encoding/json | None for v1 API; semantic changes for v2 API | Pick a config, then behavior differs | Change the import | go-json | 

go-json takes speed on both architectures. The standard library takes every row that is about risk. sonic is never the outright winner, and its case rests on amd64 and on features the other two lack.

## Hands-on: the same payload through all three

We ran the same benchmark against six configurations on macOS 27.0.1, Apple M3 Pro (arm64), Go 1.27.3, sonic v1.15.5 and go-json v0.12.0. The payload is a page of 100 orders, 91,961 bytes, with nested structs, slices, a `map[string]string`, `time.Time` values and non-ASCII strings, decoded from `[]byte` into a typed struct and encoded back.

`go test ./bench -run '^$' -bench . -benchtime=1s -count=10 -benchmem`
Setup friction was close to zero. `go get github.com/goccy/[email protected]` added one module. `go get github.com/bytedance/[email protected]` added sonic plus seven more: its loader, `cloudwego/base64x`, `klauspost/cpuid/v2`, `twitchyliquid64/golang-asm`, `bytedance/gopkg` and two `golang.org/x` modules. Both built on Go 1.27.3 without linker errors, and `sonic.APIKind` reported 1, meaning the real sonic implementation was active rather than its `encoding/json` fallback.

| Configuration (92 KB payload) | Decode, median µs | Decode vs encoding/json | Encode, median µs | Encode vs encoding/json | 
|---|---|---|---|---|
| encoding/json (v1 API) | 462 | 1.00x | 255 | 1.00x | 
| encoding/json/v2 | 377 | 1.23x | 267 | 0.96x | 
| sonic ConfigDefault | 172 | 2.69x | 252 | 1.01x | 
| sonic ConfigStd | 299 | 1.54x | 289 | 0.88x | 
| go-json (v1 API) | 188 | 2.46x | 149 | 1.71x | 
| go-json/v2 | 178 | 2.59x | 81 | 3.15x | 

Treat this as one data point from our machine. The laptop was not idle, so the interquartile ranges were wide, and we ran the suite twice. Both runs agreed that go-json encodes fastest, that sonic's encoder sits at or below `encoding/json` on arm64, and that go-json and sonic's default config decode about 2.5x to 3.6x faster than `encoding/json`. They disagreed on which of those two decodes our large payload faster (go-json at 135 µs against sonic's 162 µs in the first run), so call that a tie. On the 963-byte single-order payload, go-json decoded in 1.8 µs, sonic's default config in 2.2 µs and `encoding/json` in 6.0 µs.

The more useful output came from feeding each library three bad inputs: a duplicate key, a string with an invalid UTF-8 byte, and a key in the wrong case. This is what came back, trimmed to four of the six configurations (we also fed each one a value with HTML characters, nil slices and a map):

```
== encoding/json (v1 API)
  decode duplicate key  -> name="b" err=<nil>
  decode invalid UTF-8  -> name="a�b" err=<nil>
  decode wrong case key -> name="x" err=<nil>
  encode -> {"name":"","s":null,"m":null,"html":"\u003ca\u0026b\u003e","keys":{"a":"2","m":"3","z":"1"}} err=<nil>
== encoding/json/v2
  decode duplicate key  -> name="a" err=jsontext: duplicate object member name "name"
  decode invalid UTF-8  -> name="" err=jsontext: invalid UTF-8 within "/name" after offset 10
  decode wrong case key -> name="" err=<nil>
  encode -> {"name":"","s":[],"m":{},"html":"<a&b>","keys":{"z":"1","a":"2","m":"3"}} err=<nil>
== sonic ConfigDefault
  decode duplicate key  -> name="b" err=<nil>
  decode invalid UTF-8  -> name="a\xffb" err=<nil>
  decode wrong case key -> name="x" err=<nil>
  encode -> {"name":"","s":null,"m":null,"html":"<a&b>","keys":{"m":"3","z":"1","a":"2"}} err=<nil>
== go-json/v2
  decode duplicate key  -> name="a" err=jsontext: duplicate object member name "name"
  decode invalid UTF-8  -> name="" err=jsontext: invalid UTF-8 within "/name" after offset 10
  decode wrong case key -> name="" err=<nil>
```
sonic's default config put the raw `0xff` byte into a Go string, where `encoding/json` replaces it with U+FFFD. sonic's `ConfigStd` and go-json's v1 API behaved exactly like `encoding/json`, and go-json/v2 matched `encoding/json/v2` line for line, error text included.

## Performance: go-json leads, sonic depends on the CPU

The only cross-library benchmark on current versions is the report go-json's author publishes. It ran on 2026-11-05 on GitHub Actions with Go 1.27.3, sonic v1.15.4 and go-json at commit 476d0bb, on an AMD EPYC 9V74 with AVX-512 (amd64) and a Neoverse-N2 (arm64), with a 64 MB live heap. It groups libraries by behavior so they do the same work. Read it knowing who wrote it: it is a contender's own harness, and we could only corroborate it on arm64.

| Geometric mean speedup, 9 payloads (go-json report, 2026-11-05) | sonic amd64 | go-json amd64 | sonic arm64 | go-json arm64 | 
|---|---|---|---|---|
| Decode vs encoding/json, encoding/json behavior | 2.20x | 3.92x | 1.95x | 3.99x | 
| Encode vs encoding/json, encoding/json behavior | 2.83x | 3.49x | 1.06x | 3.17x | 
| Decode vs encoding/json/v2 defaults, v2 behavior (go-json/v2) | 1.90x | 2.61x | 1.61x | 2.80x | 
| Encode vs encoding/json/v2 defaults, v2 behavior (go-json/v2) | 3.17x | 3.52x | 1.17x | 3.36x | 

On amd64, sonic is a close second at encoding, and it beats go-json outright on a few payloads when HTML escaping is off: 157 ns against 234 ns on the small struct, 4.1 µs against 4.5 µs on the Twitter payload. On arm64 its encoder barely beats the standard library, which matches both our run and a Hacker News commenter's complaint that sonic is "still slower than std Json on arm64 with basic use cases." The cause is in sonic's source tree: the JIT compilers live only in `_amd64.go` files, and on arm64 the decoder runs sonic's non-JIT `optdec` path with NEON helpers.

Older numbers point the other way and should not be used. The Go JSON v2 authors' jsonbench last ran in January 2025 on Go 1.23.5 with sonic v1.12.7 and go-json v0.10.4, and found JSONv2 "1.3x to 1.8x slower" than GoJSON and "up to 2.8x slower" than sonic on concrete-type unmarshaling. go-json has been largely rewritten since. sonic's own README benchmarks are older still: Go 1.17.1 on an Intel i9-9880H.

Where the standard library sits: the Go 1.27 release notes say marshal performance is "broadly at parity" with the old implementation and unmarshal is "significantly faster." In our run, decoding through the v2 API was about 1.2x faster than through the v1 API on the same engine. That still leaves the v2 API roughly 2x to 2.8x behind the fastest third-party decoders, which only matters if JSON is a measurable share of your CPU.

## Strictness: only the v2 APIs reject bad input

Go 1.27's notes state that v2 "rejects invalid UTF-8 in JSON strings and rejects duplicate names within a JSON object." Duplicate keys matter when two systems parse the same document: one takes the first `"role"`, the other the last, and a gateway check passes a payload the backend reads differently. In our probes the v1 API, both sonic configs we tried and go-json's v1 API all silently kept the last value.

We found no sonic option that turns this on. go-json's report configures sonic as close to v2 as its options allow (`CaseSensitive`, `ValidateString`, `NoNullSliceOrMap` and others), and it still records "accepted invalid input" for both the duplicate-key and the invalid-UTF-8 probes. go-json/v2, new in v0.12.0, passes both.

sonic's defaults trade some safety for speed in two further places, both documented. `ConfigDefault` turns off HTML escaping and map-key sorting, so its output is not byte-identical to `encoding/json` (our probe printed `<a&b>` unescaped, keys in random order). A golden file test on the marshaled output shows that difference before you ship. And decoded strings point into the input buffer; the README says this adds memory that is "usually 20% ~ 80% of decoded objects" when you hold on to them, and offers `CopyString` to turn it off. `ConfigStd` fixes all of this at the cost of speed: 1.54x on our decode instead of 2.69x.

## Production risk: a Go version ceiling and a single maintainer

sonic pins itself to Go versions it has tested. Its `compat.go` carries this build constraint:

`//go:build (!amd64 && !arm64) || go1.28 || !go1.17 || (arm64 && !go1.20)`
When it matches, every sonic call goes to `encoding/json`. Build with Go 1.28 before sonic ships support and your code compiles, passes tests and quietly loses the speedup. The guard is one line in a test that fails when `sonic.APIKind != sonic.UseSonicJSON`. The track record is mixed. Go 1.27 support landed in v1.15.3 on 2026-08-26, seven days after Go 1.27.0, while earlier upgrades broke builds outright, as in issue #738 on Go 1.24:

`link: github.com/bytedance/sonic/loader: invalid reference to runtime.lastmoduledatap`
and, with the v2 experiment turned on, issue #891 on sonic v1.14.2:

``undefined reference to `encoding/json.unquoteBytes'``
Both are fixed in current releases, but sonic reaches into Go internals and ships native code, so each Go release is a compatibility event for it. One Hacker News commenter argued that sonic's JIT and assembly make it too large to audit ("There's no way you can audit it"), the same worry people raised when Bun's Rust rewrite merged with 13,000 unsafe blocks. ByteDance's side of the ledger: six releases so far in 2026, fixes from several contributors, and fallback behavior that is documented rather than accidental.

go-json's risk is concentrated in one person. Of the 143 commits since May 2026, 140 are by its author, and the project is still v0.x. There was a gap of more than 13 months (January 2025 to March 2026) between v0.10.5 and v0.10.6, and the Go JSON v2 authors' jsonbench recorded v0.10.4 panicking with `invalid memory address or nil pointer dereference` in its encoder VM. Then v0.11.0 (2026-09-27) removed every `go:linkname`, matched Go 1.27's decoding and error reporting, and listed behavior changes that "may change what a program which relied on the previous behavior gets." Pin the minor version and read the release notes before every bump.

The standard library carries none of this. The v1 API "will continue to be supported," and if Go 1.27's new engine breaks something you can build with `GOEXPERIMENT=nojsonv2` for now, though the notes warn that opt-out "is expected to be removed in a future release." Moving native code out of a hot path for maintainability is a trade other projects have made on purpose; Prisma 7 deleted its Rust engine and still got faster.

## Migration cost and ecosystem

go-json is the cheapest switch: change `"encoding/json"` to `"github.com/goccy/go-json"` and the behavior follows Go 1.27's `encoding/json`. The v2 packages need care. go-json/v2's types are its own, not aliases of the standard library's, so a `MarshalJSONTo` method written against `encoding/json/jsontext` is not called by go-json/v2. Replace both imports together.

sonic's API mirrors `encoding/json`, but you choose a behavior when you choose a config. Use `sonic.ConfigStd` if downstream systems compare bytes or rely on HTML escaping, and expect less of the decode gain: 1.54x over encoding/json in our run, against 2.69x for ConfigDefault. Moving from `encoding/json` to the v2 API is a semantic migration, not a speed one: case-sensitive key matching, `[]` instead of `null` for nil slices, and the rejections above. `json.DefaultOptionsV1()` restores v1 semantics on the v2 API, which lets you migrate Go encoding/json to v2 gradually instead of all at once.

On adoption, pkg.go.dev counts 6,503 importing packages for go-json and 4,037 for sonic; GitHub stars run the other way, 3,773 against 9,653. Gin v1.12.0 supports both behind build tags, `-tags=go_json` and `-tags=sonic`, so neither count settles which is more trusted. We call ecosystem a draw between the two.

## Best fit

encoding/json/v2 fits new services and anything that parses JSON from untrusted clients. Strict defaults close the duplicate-key and invalid-UTF-8 gaps with no configuration. Nobody on the team has to own a JSON dependency, and its `UnmarshalRead` and `MarshalWrite` stream instead of buffering the whole document.

go-json fits services where a CPU profile shows JSON near the top, especially fleets that run on arm64 servers. The import swap is reversible in one commit, which caps the downside of its maintenance risk.

sonic fits amd64-only fleets that already run it, for example through Gin's `sonic` tag, and code that reads a few fields out of large documents. Its `sonic.Get` and `ast.Node` APIs search partial JSON by path without decoding the whole thing, which neither alternative offers.

## Pick rules

- Pick encoding/json/v2 (or stay on the v1 API) if: JSON is under a few percent of CPU in your profiles; you parse input from parties you do not control; you want zero dependencies and no build-time surprises on the next Go release.
- Pick goccy/go-json if: JSON decoding or encoding is a measured hot spot; you deploy to arm64 or a mix of architectures; you want the speed without changing behavior, using go-json/v2 when you also want v2's strict checks.
- Pick sonic if: you deploy only to amd64; you already depend on it and have `sonic.APIKind` asserted in tests; or you need path lookups into large documents through`sonic.Get` .
- Avoid sonic's ConfigDefault if: another system validates the same payloads, because it accepts duplicate keys and passes invalid UTF-8 through as raw bytes.

## Final recommendation

Start on the standard library. Go 1.27 made `encoding/json` run on the v2 engine and made the v2 API stable, so you get faster decoding for free and the strictest input handling of the three if you adopt the v2 API. Switch to go-json only after a profile shows JSON matters. It was the fastest on both architectures in its author's report and the fastest encoder in our arm64 run, it has no dependencies, and the switch is one import that you can revert.

sonic's speed advantage is concentrated on amd64, where its JIT runs. With go-json v0.12.0 available, it wins only some small amd64 encode cases and its partial-parse API, and it brings the most build-time risk of the three.

## What would change this verdict

- An independent benchmark of go-json v0.12 against sonic on amd64. Every current amd64 number comes from go-json's own harness; a third-party run that puts sonic ahead would move the amd64 pick back to sonic.
- Go 1.28. If sonic supports it within days, as it did for 1.27, the version-ceiling risk shrinks. If it lags, sonic users fall back to `encoding/json` without an error.
- A second active maintainer or a 1.0 release for go-json, which would remove most of its risk row.
- Standard library speed work. If a future Go release closes most of the 2x to 4x decode gap, the case for any third-party library narrows to niche APIs.
