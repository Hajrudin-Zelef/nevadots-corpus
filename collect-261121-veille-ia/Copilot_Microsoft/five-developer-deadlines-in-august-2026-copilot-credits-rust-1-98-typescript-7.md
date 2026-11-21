---
id: collect-261121-veille-ia/dev-langages/five-developer-deadlines-in-august-2026-copilot-credits-rust-1-98-typescript-7
title: "Five developer deadlines in August 2026: Copilot credits, Rust 1.98, TypeScript 7"
domain: dev-langages
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-08-16"]
keywords: ["august", "credits", "copilot", "typescript", "rust", "five", "deadlines", "full"]
source: https://dev.to/akashdas/five-developer-deadlines-in-august-2026-copilot-credits-rust-198-typescript-7-1pbh
source_anchor: ""
source_lines: [1, 27]
section: "Copilot_Microsoft"
---
Five things changed for developers in the last two weeks, and four of them have a date attached. Here is the short version of each, with the full write-up linked.

Bloomberg reported the finalized deal on August 16, 2026 — roughly 5.4x the $1.3 billion valuation OpenRouter carried in May. The gateway adds no markup to inference; it charges 5.5% on card credit purchases and 5% on BYOK usage above the monthly allowance. So the question is not "is it expensive", it is "who owns the meter now".

Full breakdown, including when a direct provider key beats the gateway: Stripe bought OpenRouter. Should you still route through it?

Business pools drop from 3,000 credits per user to 1,900. Enterprise drops from 7,000 to 3,900. One credit is $0.01, so a 50-seat Business org loses $550 a month of included usage and a 200-seat Enterprise org loses $6,200 — with no change in how anyone works.

Worked numbers plus the budget controls that are off by default: Copilot's bonus credits end August 31. What breaks then?

The Go compiler landed on July 8, 2026 with 8x to 12x faster full builds — VS Code went from 125.7s to 10.6s. There is no stable programmatic API in 7.0, so typescript-eslint, ts-jest, ts-morph and the template checkers behind Vue, Svelte, Astro and Angular are stuck on TypeScript 6 until 7.1.

The staged upgrade that works today: TypeScript 7 is 10x faster. Can your project use it?

3.15.0rc1 shipped August 4, 2026, with no further ABI changes before the October 1 final. PEP 803 gives free-threaded builds a stable ABI, which is what makes C extension wheels practical. It still costs 1% to 8% of single-threaded performance, and it is still not the default build.

Which workloads actually gain, and how to test it in an afternoon: Python 3.15 hits rc1. Is free-threading ready yet?

The new fast path for `derive(PartialOrd)` when `Ord` is derived can change behaviour for types whose two ordering implementations disagree. Nothing fails to compile; your sort order moves.

Also worth correcting: several write-ups claim `c_variadic` ships in 1.98. The stabilization PR carries the **1.99.0** milestone and was merged after 1.98 branched, so it is the release after this one.

Ten-minute check for your workspace: Rust 1.98 lands August 20. One change can break crates.

Two of these have hard deadlines this month — Copilot on August 31 and Rust on August 20. The other three are decisions you can take at your own pace, as long as you take them deliberately.

## Top comments (0)
