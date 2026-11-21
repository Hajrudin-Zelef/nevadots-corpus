---
id: collect-261121-veille-ia/langages-dev/simdjson-simdjson-v5-0-6-on-github
title: "simdjson/simdjson v5.0.6 on GitHub"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["lemire", "jmestwa-coder", "ondemand", "document", "json", "stream", "simdjson", "last"]
source: https://newreleases.io/project/github/simdjson/simdjson/release/v5.0.6
source_anchor: ""
source_lines: [1, 51]
section: "Langages_Dev"
---
## What's Changed

- stop On-Demand document_stream source() at the end of an unclosed last document by @jmestwa-coder in #2936
- ondemand: split the JSONPath once in for_each_at_path_with_wildcard by @lemire in #2937
- Fix FracturedJson double column width estimate for table alignment by @jmestwa-coder in #2933
- ondemand: report DEPTH_ERROR past max_depth under development checks by @lemire in #2938
- support recursive types in static-reflection serialization by @lemire in #2939
- builder: store string_builder bytes in a std::string, add move_to() by @lemire in #2930
- pad the root-number scratch buffer in get_root_double and siblings by @jmestwa-coder in #2932
- keep threaded document_stream going after an EMPTY stage 1 batch by @jmestwa-coder in #2943
- Fix get_root_bool() accepting invalid fifth char as false by @Moaziz667 in #2940
- stop On-Demand raw_json() on a scalar document consuming the next one by @jmestwa-coder in #2941
- ondemand: end raw_json() on the last byte of the value; add raw_json() to simdjson_result<document(_reference)> by @lemire in #2944
- Count the key when deciding to inline a fractured_json object value by @ybai08 in #2942
- report a malformed JSONPath after a wildcard as INVALID_JSON_POINTER by @lemire in #2946
- Count separators when packing mixed compact multiline fractured_json rows by @ybai08 in #2945
- Write the children of an inline fractured_json value inline by @ybai08 in #2947
- ondemand: do not walk an abandoned iterator in at_pointer()/at_path() or document_stream by @anabobillo3 in #2948
- ondemand: do not wrap the token length at the end of a document_stream by @lemire in #2955
- doc: strict aliasing under Visual Studio and clang-cl by @lemire in #2956
- stage 1: end a document_stream batch between two documents by @lemire in #2954
- fix ondemand field::escaped_key() after the value is consumed by @jmestwa-coder in #2952
- builder: do not keep the whole buffer when moving out a small result by @lemire in #2957
- fallback: end a document_stream batch between two documents by @lemire in #2958
- Remove dead code; fix json_builder.h guard and get_values on error by @lemire in #2959
- Fix Windows CI after the move_to() change by @lemire in #2960
- dom: keep the prettify indentation past 16 levels of nesting by @lemire in #2961
- Fix finished batch-end documents, stars in the last JSONPath key, and move_to exceptions by @lemire in #2963
- dom: write integers with the builder's jeaiii routine by @mvanslobbe in #2962
- test: prettify the element itself when exceptions are off by @lemire in #2964
- Avoid a bogus GCC 12 -Wrestrict error in integer_test by @lemire in #2965
- Remove mention of Awesome Modern C++ list. by @RealTimeChris in #2966
- write big integers in fractured_json instead of dropping them by @jmestwa-coder in #2967
- ondemand: do not match a key ending in a backslash on an escaped quote by @jmestwa-coder in #2971
- dom: return INSUFFICIENT_PADDING from unpadded parse_many input by @jmestwa-coder in #2970
- unquote bracket keys after the last wildcard in at_path_with_wildcard by @jmestwa-coder in #2968
- ondemand: leave the trailing-backslash scan off the key fast path by @lemire in #2972
- cmake: fix checkperf target dependencies under Ninja by @Hrushikesh-ramilla in #2977
- ondemand: do not reject root numbers over trailing whitespace, quotes, or stream delimiters by @lemire in #2979
- cmake: repair checkperf on Makefiles and Ninja by @lemire in #2978
- ondemand: skip nested values with a depth-delta table by @mvanslobbe in #2976
- stage 1: build the last block without a store-forwarding stall by @mvanslobbe in #2969

## New Contributors

- @ybai08 made their first contribution in #2942
- @anabobillo3 made their first contribution in #2948
- @RealTimeChris made their first contribution in #2966
- @Hrushikesh-ramilla made their first contribution in #2977

**Full Changelog**: `v5.0.5...v5.0.6`
