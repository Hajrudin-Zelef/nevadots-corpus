---
id: collect-261121-veille-ia/zig-lang/zig-release-history-0-17-and-the-road-to-a-stable-language
title: "Zig release history: 0.17 and the road to a stable language"
domain: zig-lang
role: reference
task: article
actors: []
dates: []
keywords: ["stable", "release", "language", "notes", "releases", "since", "october", "zig"]
source: https://dev-news.anonyfox.com/zig/
source_anchor: ""
source_lines: [1, 25]
section: "Zig_Lang"
---
Latest stable Zig: **`0.17.1`**, released  (0.17.1 release notes). 10 stable releases since 20 Apr 2024.

# Zig

**0.17.0**Released 1 October 2026 · 925 commits · 206 contributors

- **10** stable releases since Apr 2024
- **77** days between stable releases, median
- **2** in Jun 2024, the busiest month
- **270** days, the longest wait (8 Jun 2024 to 5 Mar 2025)

## Removed in 0.17

- ~~[1]u8{0} ** n~~`@splat(0)`
- ~~@cImport(...)~~`translate_c package`
- ~~void{}~~
- ~~errdefer |err|~~
- ~~i0~~
- ~~.internal / .link_once~~

Zig 0.17.0 shipped on 1 October 2026 after five months of work: 925 commits from 206 contributors. Since 0.16.0 the core team has decided on roughly 150 language proposals, accepting around 25 and rejecting around 125, as part of settling the language before 1.0. When the release notes were written, 23 undecided proposals were still open on Codeberg and 61 on the legacy GitHub tracker.

The release removes @cImport, deprecated in 0.16, in favour of the separate translate-c package, and drops array multiplication syntax, void{}, errdefer captures and i0. zig build now runs build.zig in its own configurer process, apart from a maker that is built once with optimizations, and incremental compilation with -fincremental --watch is expected to work for most projects targeting x86_64-linux.

As of 24 October 2026. Zig 0.17.0 release notes.
