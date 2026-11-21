---
id: collect-261121-veille-ia/langages-dev/simdjson-simdjson-v5-0-7-on-github
title: "simdjson/simdjson v5.0.7 on GitHub"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simdjson", "lemire", "github", "drop", "jmestwa-coder", "past", "stream", "many"]
source: https://newreleases.io/project/github/simdjson/simdjson/release/v5.0.7
source_anchor: ""
source_lines: [1, 9]
section: "Langages_Dev"
---
## What's Changed

- stage 1: do not drop the RS before an unclosed json_sequence string by @jmestwa-coder in #2980
- ondemand: drop redundant per-type smart-pointer deserializers by @lemire in #2982
- do not read past a view whose padding is trailing white space by @jmestwa-coder in #2983
- doc: guidance for parsing a stream on many threads by @lemire in #2987
- dom: fix parse_many reading past the buffer on truncated stream batches by @lemire in #2988

**Full Changelog**: `v5.0.6...v5.0.7`
