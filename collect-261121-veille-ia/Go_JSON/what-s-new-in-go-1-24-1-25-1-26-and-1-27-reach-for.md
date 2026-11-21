---
id: collect-261121-veille-ia/go-json/what-s-new-in-go-1-24-1-25-1-26-and-1-27-reach-for
title: "What's new in Go 1.24, 1.25, 1.26 and 1.27 — Reach For"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["type", "release", "generic", "code", "notes", "change", "uuid", "full"]
source: https://www.reachfor.dev/whats-new-in-go
source_anchor: ""
source_lines: [1, 47]
section: "Go_JSON"
---
# What's new in Go, 1.24 → 1.27

Four releases, two years of changes, and most of them you can ignore. These are the three per release that change how you write Go day to day, each with code you can paste and run. Newest first.

## Go 1.27 · August 2026

Methods get their own type parameters, and JSON gets a v2.

1. ### 01Generic methodsA method can now declare type parameters of its own, not just use its receiver's. Helpers that had to be package-level functions can live on the type. The standard library already uses it: `(*rand.Rand).N` in`math/rand/v2` . Interface methods still can't be generic.
2. ### 02encoding/json/v2 is a real packageThe JSON rewrite that was behind `GOEXPERIMENT=jsonv2` in 1.25 is now a standard package, with stricter defaults. Two that change real programs: a nil slice encodes as`[]` instead of`null` , and object keys match struct fields case-sensitively. The same code under v1 prints`"tags":null` and fills`Name` with`"x"` .
3. ### 03uuid in the standard libraryA new `uuid` package generates and parses RFC 9562 UUIDs, so one of the most common third-party imports is gone.`uuid.New()` is the general-purpose choice;`uuid.NewV7()` gives time-ordered IDs, which index well as database keys.

Full list: Go 1.27 release notes.

## Go 1.26 · February 2026

Smaller syntax wins, and a new garbage collector by default.

1. ### 01new(expr)`new` now takes an expression as well as a type, so a pointer to a value is one call. The everyday case: optional fields as pointers, which used to need a temporary variable or a helper like`func ptr[T any](v T) *T` .
2. ### 02errors.AsTypeA generic, type-safe `errors.As` : no target variable declared up front, no pointer-to-a-pointer, and the match is scoped to the`if` .
3. ### 03Green Tea GC is on by defaultThe garbage collector that was an experiment in 1.25 is now the default. It marks and scans small objects with better memory locality, and the release notes expect 10–40% less GC overhead in programs that collect a lot. Nothing to change in your code; opt out with `GOEXPERIMENT=nogreenteagc` if you measure a regression.

Full list: Go 1.26 release notes.

## Go 1.25 · August 2025

Concurrency gets easier to write and to test.

1. ### 01sync.WaitGroup.Go`wg.Go(f)` does`Add(1)` , starts the goroutine and calls`Done` for you, which removes the classic bug of calling`Add` inside the goroutine. Combined with per-iteration loop variables (Go 1.22), the closure below is safe as written.
2. ### 02testing/synctestTest time-dependent concurrent code without real waiting. Inside `synctest.Test` , goroutines run in a bubble with a fake clock that jumps forward whenever every goroutine is blocked. The test below sleeps for ten minutes and finishes in milliseconds.
3. ### 03Container-aware GOMAXPROCSOn Linux, `GOMAXPROCS` now defaults to the lower of the machine's CPUs and the container's cgroup CPU limit, and updates if the limit changes. A service limited to 2 CPUs on a 64-core node no longer runs 64 threads' worth of parallelism into throttling. No code change.

Full list: Go 1.25 release notes.

## Go 1.24 · February 2025

A new map, generic aliases, and honest benchmarks.

1. ### 01Maps are Swiss TablesThe built-in `map` was reimplemented as a Swiss Table: slots in groups of eight, each with a one-byte control word. A lookup checks a whole group's tags in one operation and compares keys only where a tag matches. Same API, faster maps; step through a lookup:
2. ### 02Generic type aliasesA type alias can take type parameters, like a defined type. Useful for short names over generic types without creating a new, distinct type.
3. ### 03testing.B.Loop`for b.Loop()` replaces`for i := 0; i < b.N; i++` . Setup before the loop runs once and is not timed, and the compiler can't optimise away the work inside it, which made the old form easy to get wrong.

Full list: Go 1.24 release notes.

## Learning DSA in current Go

Reach For teaches data structures, algorithms and system design for interviews in modern Go: every code example in the course is compiled and run against Go 1.27 before it ships. Start with the free modules, or see why the course uses Go.
