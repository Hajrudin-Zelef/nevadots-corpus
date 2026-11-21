---
id: collect-261121-veille-ia/langages-dev/zig-0-17-the-grammar-starts-to-freeze-and-the-build-system-splits-in-two-moiu-home
title: "Zig 0.17: the grammar starts to freeze and the build system splits in two | MoIu Home"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-02"]
keywords: ["build", "system", "grammar", "removed", "release", "syntax", "changes", "notes"]
source: https://www.9100.ink/en/blog/zig-017/
source_anchor: ""
source_lines: [1, 47]
section: "Langages_Dev"
---
Zig 0.17.0 landed on October 2: **five months, 206 contributors, 925 commits**. The release notes open with a very Zig sentence â âoriginally predicted to be shorter, this release cycle ended up substantialâ.

## Language level: start with what breaks compilation

The keyword for 0.17 is **language stability progress**: the grammar is now formally specified and fuzz-tested, and C translation (`zig translate-c`) moved out of the compiler into an external package. That is the project starting to answer âwill the syntax keep moving under meâ.

The changes that break builds (walk this list when upgrading):

- `\@bitCast` semantics changed (the sneakiest one: it does not report âundefinedâ, it quietly changes behaviour)
- **array multiplication syntax removed**
- `void{}` syntax removed
- `errdefer` capture form removed
- `i0` type removed
- `internal` and`link_once` global linkage removed
- `\@hasDecl` now returns`true` only for**public** declarations

Additions lean low-level: `\@backingInt` / `\@fromBackingInt` (the backing integer of enums and structs), `\@SpirvType`, `\@divCeil`.

## Standard library: renames hurt more than features

- **`SafeAllocator` arrives** ;`StackFallbackAllocator` is reworked and renamed
- **`ArrayList` pointer stability** : the long-standing pain where growth invalidates slice pointers now has a guarantee
- `debug.SafetyLock` gains shared locking
- `fmt.allocPrint` moves onto`mem.Allocator`
- `std.zon.parse` reworked
- `bit_set` gains a struct-of-arrays variant; the managed variant is deprecated
- `lang.OptimizeMode` renamed to`lang.Optimize`

## Build system: maker and configurer split

The structural change is in the build system: the process that **produces the build graph** and the one that **configures** are now separate, the cache system is reworked, and the concept of **configure cache poisoning** is introduced (once the configuration cache is poisoned, results cannot be trusted). **Build Server Protocol** support landed too.

For end users this means faster `zig build`; for people writing build scripts it means a lot of rewriting. The notes are honest that this release **contains known bugs** and list notable regressions.

## Compiler and backends

Incremental compilation keeps progressing, LLVM moves to **22**, the aarch64 / loongarch / WebAssembly backends all changed, SPIR-V is being filled in, and the linkers (ELF / COFF) got a new testing framework.

## The judgement

0.17 is âslowâ because it is paying down debt: freezing the grammar, externalising C translation, splitting the build system into processes â all prepayment for fewer changes later. Practical advice: on upgrade, run `zig build` first and read the syntax error list, then work through the changes like `\@bitCast` and `\@hasDecl` that **do not error but do change meaning**.

*Source: Zigâs â0.17.0 Releasedâ (2026-10-02) and the 0.17.0 release notes.*

## Comments

â¦
