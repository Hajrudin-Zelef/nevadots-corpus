---
id: collect-261121-veille-ia/langages-dev/simdjson-simdjson-v5-1-1-on-github
title: "simdjson/simdjson v5.1.1 on GitHub"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lemire", "simdjson", "fractured", "json", "ybai08", "github", "indentation", "supersedes"]
source: https://newreleases.io/project/github/simdjson/simdjson/release/v5.1.1
source_anchor: ""
source_lines: [1, 16]
section: "Langages_Dev"
---
## What's Changed

- prettify: configurable indentation step, faster indentation (supersedes #2916) by @lemire in #2994
- README: add Apache Arrow to real-world usage by @lemire in #2993
- keep the parser's settings in threaded parse_many and iterate_many by @jmestwa-coder in #2996
- ondemand: user-provided value() constructor to avoid store-forwarding stalls with GCC 15/16 by @lemire in #2999
- Do not end fractured_json lines with alignment padding by @ybai08 in #2995
- Count fractured_json separators without padding when it is off by @ybai08 in #3000
- Write the containers in fractured_json table cells inline by @ybai08 in #3001
- dom: escape strings with the builder's SIMD escaper (supersedes #2997) by @lemire in #2998
- dom: do not stop at a null character in at_key_case_insensitive by @jmestwa-coder in #3003
- cmake: build the benchmarks that need counters only when it is available by @lemire in #3006
- cmake: offline developer builds work as packaged, and are tested in CI by @lemire in #3007
- CI: fix the offline workflow (duplicate env keys) by @lemire in #3008

**Full Changelog**: `v5.1.0...v5.1.1`
