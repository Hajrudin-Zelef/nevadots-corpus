---
id: collect-261121-veille-ia/langages-dev/go-1-27-adds-generic-methods-and-a-faster-encoding-json
title: "Go 1.27 adds generic methods and a faster encoding/json"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["json", "type", "release", "methods", "encoding", "generic", "method", "goroutine"]
source: https://dev.to/techaiwire/go-127-adds-generic-methods-and-a-faster-encodingjson-4d2h
source_anchor: ""
source_lines: [1, 55]
section: "Langages_Dev"
---
Go 1.27 was released on August 19, 2026, and it changes the language itself. Methods can now declare their own type parameters, which Go has refused since generics arrived. Nicholas Husin announced the release on the Go blog for the Go team.

Two other changes will reach more programs than the language work does. The standard JSON package now runs on a new engine, and the runtime can tell you which goroutines have leaked.

Before this release, a type could be generic but its methods could not add type parameters of their own. Authors wrote one method per concrete type instead.

The release notes point at `math/rand/v2.Rand` as the example. Its method now reads `(*Rand) N[Int intType](Int) Int`. One method covers every integer type.

There is a firm restriction. Interface methods may not declare type parameters, so a generic method cannot implement an interface method. That boundary is deliberate and it is not a temporary gap.

Two smaller language changes ship alongside it. A key in a struct literal "may now be any valid field selector for the struct type," which lets you set nested and embedded fields directly. And function type inference "has been generalized to apply in all assignment contexts", including composite literals, conversions and channel sends.

The `encoding/json` package is now backed by a v2 implementation. The v1 API stays, and the release notes are explicit that it is not deprecated. No migration is required.

The defaults did get stricter, and this is where working code can break. The v2 engine rejects invalid UTF-8 inside JSON strings. It also rejects duplicate names in a JSON object. Both used to pass.

| Change | Effect | 
|---|---|
| Invalid UTF-8 in strings | Now rejected | 
| Duplicate object keys | Now rejected | 
| Unmarshal speed | Significantly faster | 
| Marshal speed | About the same | 
| `format` and`unknown` tags | Removed in v2 | 
| `inline` tag option | Renamed to `embed` | 

If something breaks, `GOEXPERIMENT=nojsonv2` restores the old implementation at build time. Treat that as a deadline, not a fix. The release notes say the opt-out is expected to be removed in a future release.

Two new packages come with it: `encoding/json/v2`, which takes variadic options, and `encoding/json/jsontext` for working at the token level.

The goroutine leak profile is now generally available in `runtime/pprof`, after being experimental in Go 1.26. A leaked goroutine is "a goroutine blocked on some concurrency primitive (channels, sync.Mutex, sync.Cond, etc) that cannot possibly become unblocked".

The runtime finds them using the garbage collector. If a blocked goroutine waits on something no runnable goroutine can still reach, it can never wake. Read it at `/debug/pprof/goroutineleak`. It has a stated blind spot: leaks on primitives reachable through global variables can be missed.

Allocation got cheaper. The compiler now calls size-specialized allocation routines, cutting the cost of allocations under 80 bytes by up to 30%. The release notes put the whole-program gain at roughly 1% for allocation-heavy code, and the binary grows by about 60 KB.

For security teams, `crypto/mldsa` implements ML-DSA, the post-quantum signature scheme in FIPS 204. TLS 1.3 gains the MLDSA44, MLDSA65 and MLDSA87 signature schemes, and x509 handles the keys.

Read the removals before you upgrade, because that is where the breakage is. Go 1.27 now requires macOS 13 Ventura or later. It drops support for the bzr version control system, so modules hosted on bzr can no longer be fetched.

Several GODEBUG escape hatches are gone, and one of them changes behavior. With `asynctimerchan` removed, channels created by package `time` are always unbuffered now. If you wrote code around the buffered behavior, that code is running on a different contract.

The JSON change deserves a staging run rather than a reading. Stricter parsing fails on data you already accept, and duplicate keys in particular tend to arrive from other people's systems, not your own tests. Point the new build at real production payloads before you ship it.

Turn on the leak profile even if nothing looks wrong. It is free, it is now stable, and a permanently blocked goroutine holds its stack and everything the stack references. That class of bug rarely shows up as an error, only as memory that never comes back.

One last check for anyone doing clever things with function values. The compiler generates simpler names for closures now, so tests that assert on symbol names may need updating. Code that compares function pointers for equality was always incorrect, and this release exposes it more often.

*This article was first published on Tech AI Wire.*

Deutsch · 日本語 · Français · Español · Português

- Go 1.27 is released - The Go Blog
- Go 1.27 Release Notes - go.dev

## Top comments (0)
