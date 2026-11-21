---
id: collect-261121-veille-ia/langages-dev/go-team-confirms-encoding-json-v2-and-jsontext-are-stable-in-go-1-27
title: "Go Team Confirms encoding/json/v2 and jsontext Are Stable in Go 1.27"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "team", "encoding", "jsontext", "package", "packages", "empty", "stable"]
source: https://vinish.dev/go-json-v2-stable-in-go-1-27
source_anchor: ""
source_lines: [1, 66]
section: "Langages_Dev"
---
The Go team published a new post on the official Go blog on November 4, 2026, confirming that two JSON packages, encoding/json/v2 and its companion encoding/json/jsontext, are now stable, generally available parts of the standard library in Go 1.27. The packages started life as an opt-in experiment in Go 1.25 and went through more community feedback before being promoted.

JSON handling is in almost every Go service, so this matters. The new packages fix long-standing correctness gaps in the original encoding/json package. They also add a true streaming design and, according to the Go team, can unmarshal data up to ten times faster in some benchmarks.

## What Changed

Go 1.27.0 shipped on August 19, 2026, and the two new packages arrived with it as regular, non-experimental APIs. The new blog post is a refreshed version of the earlier article that introduced the experiment. It explains the reasoning behind the redesign and points developers to migration material.

The Go team split JSON work into two layers. The jsontext package deals only with JSON syntax: reading and writing tokens and raw values. The json/v2 package sits above it and maps JSON values to Go values and back.

The original encoding/json package is not going away. It stays covered by the Go 1 compatibility promise, and the team says programs using it never have to switch.

## Key Behavior Differences From encoding/json

The redesign mainly targets defaults that caused silent bugs or security problems in the old package. The most notable changes in v2 are:

- Input that contains invalid UTF-8 now produces an error instead of passing through quietly.
- A JSON object with the same member name twice is rejected, which closes a known route for parser confusion attacks.
- Nil slices and nil maps are written as an empty array or empty object rather than null.
- Matching of JSON member names to struct fields is case-sensitive during unmarshaling.
- The omitempty tag now skips a field when it would encode as null, an empty string, an empty array or an empty object.
- Marshaling a time.Duration fails by default, since it has no agreed JSON form, and options let the caller pick one.

Almost every one of these defaults can be switched back with a struct tag or a caller option. That lets teams adopt v2 gradually without breaking clients that rely on old behavior.

## How the New API Works

The top-level functions look familiar. Marshal and Unmarshal keep a similar shape but now accept variadic options. New MarshalWrite and UnmarshalRead functions work directly with an io.Writer or io.Reader, and MarshalEncode and UnmarshalDecode work with the jsontext Encoder and Decoder types.

Custom types get two new interfaces, MarshalerTo and UnmarshalerFrom. Their methods receive an encoder or decoder instead of a byte slice, so a type can stream its output and receive options from the caller. The older MarshalJSON and UnmarshalJSON methods still work in v2.

Callers can also override how any type is handled with WithMarshalers and WithUnmarshalers. These take precedence over methods defined on the type, which is useful when you cannot change a type from another package, such as generated protobuf messages.

## Performance and Compatibility

The Go team reports that marshaling speed is about the same as v1, sometimes a little faster and sometimes a little slower. Unmarshaling is where v2 shines, with benchmark gains of up to ten times. The team says more tuning for the slower paths is planned for Go 1.28.

The biggest wins come when existing custom marshalers also implement the streaming interfaces. The post cites a Kubernetes OpenAPI parsing case where switching to the streaming method gave a speedup of several orders of magnitude.

Interoperability was a design goal. Under the hood, v1 is now built on top of v2 with a full set of compatibility options. Packages using either version can therefore call each other's custom methods, so one dependency choosing v2 does not force the rest of a project to move.

## What It Means for Developers

If you build APIs, CLIs or data pipelines in Go, json/v2 is worth a look now that it is stable. The stricter defaults catch malformed or ambiguous input at the edge of your service. The faster decoding path can lower CPU use in JSON-heavy services.

Be careful with the changed defaults, though. Code that relied on case-insensitive field matching, on null for empty slices, or on duplicate keys being tolerated may behave differently after a direct switch. Clients written in other languages may also notice that empty arrays replace null values.

A safe approach is to migrate one call site at a time. Start with internal services, run your tests, and use the compatibility options where a client expects the old output.

For custom types on hot paths, add the streaming methods next to the existing ones. Developers comparing language ecosystems can see where Go fits in our guide to the best programming language to learn in 2026.

## How to Get Started

Upgrade the toolchain to Go 1.27 or later; the current point release in that line is the safest choice because it carries recent security fixes. Then import encoding/json/v2 and encoding/json/jsontext where needed. No experiment flag is required anymore.

The Go project points developers to its Working with JSON tutorial, the package documentation, and a dedicated v2 migration guide. The documentation for encoding/json lists each behavior difference together with the option that controls it.

## Conclusion

After roughly six years of design work, Go now has a modern JSON stack in the standard library that is stricter, streams properly and decodes much faster. The old package remains fully supported, so teams can adopt the new one at their own pace while gaining safer defaults where they need them most.

## Sources

**Get more Vinish.dev tutorials on Google**

See my articles more often when you search Google.
