---
id: collect-261121-veille-ia/langages-dev/go-1-27-ships-encoding-json-v2-five-silent-defaults-in-the-old-package-that-the-new-one-turns-into-e
title: "Go 1.27 Ships encoding/json/v2: Five Silent Defaults in the Old Package That the New One Turns into Errors"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-11-04"]
keywords: ["json", "encoding", "package", "team", "defaults", "figure", "error", "name"]
source: https://blog.zxce3.net/posts/go-encoding-json-v2-go-1-27-strict-defaults-duplicate-keys-utf8/
source_anchor: ""
source_lines: [1, 54]
section: "Langages_Dev"
---
The Go team announced on 4 November that `encoding/json/v2` and `encoding/json/jsontext` are stable and generally available in Go 1.27. The package has been an experiment for over a year, so the news is less about new features and more about a decision: the standard library now has a second JSON package whose defaults are stricter than the first. The interesting question for most teams is what v1 has been quietly accepting.

## What v1 Accepts Without Complaint#

I wrote a small probe against the current `encoding/json` (Go 1.24 on my machine) using five inputs the Go team lists among v1’s behavior flaws:

*Figure 1: Terminal output from my run of v1. Every case succeeds; none returns an error.*

Each line is a small trap. A `nil` slice becomes `null`, which stricter consumers reject where they expect an array. The key `"NAME"` fills a field called `Name`. A duplicate key silently keeps the last value, a pattern the Go team ties to CVE-2017-12635, where two parsers disagreed about the same document. Invalid UTF-8 is replaced rather than rejected, even though RFC 8259 requires valid UTF-8. And `Decoder.Decode` happily ignores trailing junk after the first value.

## What v2 Changes#

v2 reports an error for invalid UTF-8 and duplicate names, marshals nil slices and maps as `[]` and `{}`, and matches struct fields case-sensitively. It also redefines `omitempty` to omit anything that would encode as `null`, `""`, `[]` or `{}`, and refuses to serialize `time.Duration` without an explicit choice.

| Input | v1 (measured above) | v2 (per Go team) | 
|---|---|---|
| Duplicate member name | last value wins | error | 
| Invalid UTF-8 | accepted | error | 
| `"NAME"` into`Name` | matches | no match | 
| nil slice or map | `null` | `[]` /`{}` | 

*Figure 2: v1 versus v2 defaults. The v2 column comes from the Go team’s post; I could not run v2 on my Go 1.24 toolchain.*

Most of these have a struct tag or call option that restores v1 semantics, so migration can be incremental rather than a flag day.

## The Architecture Behind It#

The design splits syntax from meaning. `jsontext` handles grammar and streaming without reflection; `json/v2` maps values to Go types on top of it.

*Figure 3: My simplified view of the layering described in the Go post. Options flow through every layer.*

That split is what enables the new `MarshalerTo` and `UnmarshalerFrom` interfaces, which let a type write or read tokens directly instead of returning a byte slice that the package must re-validate and re-parse:

*Figure 4: The streaming hook and the reader-based entry point, as listed in the Go post.*

The payoff is performance and correctness. Per the Go team, v2 `Unmarshal` is up to 10x faster in their benchmarks, while `Marshal` is roughly at parity. I did not reproduce those numbers, and the largest wins require types to adopt the new interfaces. Options also reach custom methods, fixing the v1 problem where `DisallowUnknownFields` stops working once a custom `UnmarshalJSON` runs.

## Key Takeaways#

- v1 accepts duplicate keys, invalid UTF-8, trailing data and case-mismatched names without an error.
- v2 rejects the first two by default and uses case-sensitive matching, so test API payloads before switching.
- Expect `null` to become`[]` or`{}` in output that other systems parse.
- Unmarshal speedups depend on adopting `UnmarshalerFrom` , not just changing the import.
- Treat stricter parsing as a security improvement, but verify behavior with your own inputs.

## Conclusion#

The practical move is not a rewrite. Run your existing JSON fixtures through both packages, diff the results, and decide per endpoint whether the stricter behavior is a bug caught or a contract broken. The inputs in Figure 1 make a good starting test suite.

## References#

- A new Go API for JSON — The Go Blog, 4 November 2026
- RFC 8259: The JSON Data Interchange Format — IETF
- CVE-2017-12635 — NVD
