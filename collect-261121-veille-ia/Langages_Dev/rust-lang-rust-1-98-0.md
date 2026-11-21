---
id: collect-261121-veille-ia/langages-dev/rust-lang-rust-1-98-0
title: "rust-lang/rust 1.98.0"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-10-25"]
keywords: ["source", "rust", "rust-lang", "article", "cited", "long", "derive", "cell"]
source: https://rubric.hrih.si/editions/2026-10-25/4/
source_anchor: ""
source_lines: [1, 52]
section: "Langages_Dev"
---
Rust 1.98.0 lets &mut lifetimes shorten during unsize coercion, even in invariant positions such as Cell<&'long mut i32> to Cell<&'short mut dyn Send>. It adds lints for faulty definitions of runtime symbols like memcmp and strlen, one deny-by-default. Five thumbv7/v8 bare-metal targets move to Tier 2. Compatibility changes may break builds: derive(PartialOrd) now takes a fast path when Ord is derived, which affects inconsistent impls. Emscripten always uses WASM exception handling, and Windows thread-local destructors use Fiber Local Storage. Some of these items could not be independently verified.

## Why it ranked

No. 4 of 93 with a score of 0.69. The panel rated it highest on new information (0.95) and substantiated (0.82), lowest on significance (0.63).

Every answer behind this is in the table under Score.

## Score

**0.69**total under rubric v2.2

| Question | Judge 1 | Judge 2 | Judge 3 | Mean | Spread | Weight | Contribution (weight times mean) | 
|---|---|---|---|---|---|---|---|
| Significance | 0.66 | 0.60 | 0.62 | 0.63 | 0.06 | +0.24 | +0.151 | 
| Reader interest | 0.50 | 0.85 | 0.78 | 0.71 | 0.35 | +0.26 | +0.185 | 
| Audience breadth | 0.64 | 0.80 | 0.58 | 0.67 | 0.22 | +0.13 | +0.087 | 
| Practical relevance | 0.76 | 0.75 | 0.70 | 0.74 | 0.06 | +0.22 | +0.162 | 
| Substantiated | 0.72 | 0.85 | 0.90 | 0.82 | 0.18 | +0.05 | +0.041 | 
| Technical depth | 0.69 | 0.55 | 0.72 | 0.65 | 0.17 | +0.05 | +0.033 | 
| New information | 0.96 | 0.90 | 1.00 | 0.95 | 0.10 | +0.05 | +0.048 | 
| Promotional | 0.04 | 0.05 | 0.02 | 0.04 | 0.03 | −0.19 | −0.007 | 
| Overclaim | 0.10 | 0.05 | 0.02 | 0.06 | 0.08 | −0.11 | −0.006 | 
| Sum of contributions |  |  |  |  |  |  | +0.693 | 
| Claim-check penalty |  |  |  |  |  |  | 0.000 | 
| Total, as published |  |  |  |  |  |  | 0.693 | 

Spread is the highest minus the lowest judge answer. marks a spread above 0.40: the judges disagree, and the story is marked contested.

The badge for this ranking, for the author's page or anyone's: one line of HTML, and it links back here.

`<a href="https://rubric.hrih.si/editions/2026-10-25/4/"><img src="https://rubric.hrih.si/badge/2026-10-25/4.svg" alt="Ranked #4 on Rubric, 25 Oct 2026" width="238" height="28"></a>`
## Claim check

Claim check: 2 matches source · 1 no source given · 5 couldn’t check. Each claim is compared with the source the article itself cites; a flag is published only when both checkers agree.

1. Matches sourceVersion Rust 1.98.0 allows shortening the lifetime of &mut when unsize-coercing, even in an invariant position, e.g. coercing Cell<&'long mut i32> to Cell<&'short mut dyn Send>. The cited source says the same thing. Source cited by the article: github.com/rust-lang/rust/pull/149219
2. Matches sourceVersion Rust 1.98.0 adds a deny-by-default invalid_runtime_symbol_definitions lint and a warn-by-default suspicious_runtime_symbol_definitions lint. The cited source says the same thing. Source cited by the article: github.com/rust-lang/rust/pull/155521
3. Couldn’t checkVersion Rust 1.98.0 promotes thumbv7a-none-eabi, thumbv7a-none-eabihf, thumbv7r-none-eabi, thumbv7r-none-eabihf and thumbv8r-none-eabihf to Tier 2. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/rust-lang/rust/pull/155763
4. Couldn’t checkDate The derive macro at {core,std}::derive was unintentionally stabilized in Rust 1.96 and is now explicitly accepted as a stabilized API. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/rust-lang/rust/issues/159856
5. No source givenVersion The MSRV for {core,std}::derive will be 1.96, not 1.98. The article gives no link or citation for it.
6. Couldn’t checkCausal conclusion The new derive(PartialOrd) fast path when deriving Ord can break crates whose PartialOrd and Ord impls were inconsistent with each other. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/rust-lang/rust/pull/155598
7. Couldn’t checkVersion On Emscripten the WASM exception handling ABI is now unconditionally used, and the -Zemscripten-wasm-eh=false flag to switch back to JS exceptions has been removed. The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/rust-lang/rust/pull/156928
8. Couldn’t checkVersion The destructor implementation for thread locals on Windows now uses Fiber Local Storage (FLS). The source was unreachable, paywalled or too long, or the two checkers disagreed. Source cited by the article: github.com/rust-lang/rust/pull/148799

## Also covered by

## Elsewhere

Elsewhere: HN —

Recorded for context only, checked 25 Oct 2026, 05:46 UTC. It played no part in the ranking.
