---
id: collect-261121-veille-ia/go-json/pattern-matching-enum-proposals
title: "Pattern matching + Enum proposals"
domain: go-json
role: reference
task: article
actors: []
dates: []
keywords: ["protocol", "matching", "enum", "mojo", "tagless", "performance", "calculate", "volume"]
source: https://forum.modular.com/t/pattern-matching-enum-proposals/3473/71
source_anchor: ""
source_lines: [1, 57]
section: "Go_JSON"
---
@lukas Ok, I’ve published my cleaned-up enum benchmark here. It looks like GPT has found us a new matching protocol (i.e. a new `EnumLike`) that is up to **2x more efficient** than the one shipped in Mojo 1.2. This matching protocol greatly improves the performance of the tagless enum encodings we’ve been discussing, while preserving the performance of tag-based encodings.

Conclusions we can draw:

- It looks like we can improve the performance of tagless enum encodings by a significant margin. And the new protocol appears simple to implement; I haven’t found any tradeoffs yet.
- The performance improvements reinforce that `enum` shouldn’t provide the guarantee that its payloads have the same layout as a`Tuple` . (Reminder: In my earlier posts, I explained how the`Shape` ’s tagless representation is incompatible with the`Tuple` -based model.)

GPT ran the benchmark for me. The benchmark code is here. Here are the results on my Apple M4.

- Hardware: base Apple M4, 4 performance cores plus 6 efficiency cores, 16 GiB RAM.
- Compiler: Mojo 1.2.0 (2016d0cd), **`-O3 -D ASSERT=none`** .
- Dataset: **16,777,216 shapes** (`2**24` ), seed 42, four cases, dimensions from 1 through 1000.
- Two separate program runs, each with 21 timed repetitions and two untimed warmups per variant.
- Tagless `Shape` :**12 bytes** per value,**192 MiB** per input array. Tagged`Shape2` :**16 bytes** ,**256 MiB** . Tagless storage uses 25% less memory.

Reproduce with:

```
mojo build -O3 -D ASSERT=none shape_benchmarks.mojo -o shape_benchmarks
./shape_benchmarks 24 21 10
```

Times below are in milliseconds, averaging the two runs’ medians. Percentages are calculated from those averaged times.

`calculate_volume` sums box volumes or rectangle areas. `count_boxes` counts Box cases. The matching protocol labelled “Mojo 1.2” uses integer-discriminant matching (`_get_enum_discriminant`); the “New” protocol is an alternative pattern-matching protocol built upon a proposed new `Enumlike` method (`_is_enum_case`). The benchmark code defines the new method; I suggest having a look at it.

## Single-threaded calculation

| Workload | Matching protocol | Tagless (ms) | Tagged (ms) | Throughput difference | 
| `calculate_volume` | Mojo 1.2 | 15.881 | **9.572** | -39.7% | 
| `calculate_volume` | **New protocol** | **8.214** | 9.685 | +17.9% | 
| `count_boxes` | Mojo 1.2 | 5.331 | **4.503** | -15.5% | 
| `count_boxes` | **New protocol** | 5.380 | **4.498** | -16.4% | 
| `copy` | N/A | **6.925** | 7.068 | +2.1% | 

 ## Multi-threaded calculation

This is the same workload as for the single-threaded case, but now we have 10 hardware threads doing work in parallel. This saturates my M4’s memory bandwidth, making the tagless encoding (i.e. the smaller encoding) the clear winner when the new matching protocol is used.

| Workload | Matching protocol | Tagless (ms) | Tagged (ms) | Throughput difference | 
| `calculate_volume` | Mojo 1.2 | 2.879 | **2.587** | -10.1% | 
| `calculate_volume` | **New protocol** | **1.932** | 2.474 | +28.0% | 
| `count_boxes` | Mojo 1.2 | **1.854** | 2.412 | +30.1% | 
| `count_boxes` | **New protocol** | **1.838** | 2.417 | +31.5% | 
| `copy` | N/A | **4.663** | 6.093 | +30.7% | 

 ## Effect of the new matching protocol

This table compares the performance of the new matching protocol (`_is_enum_case`) against the Mojo 1.2 matching protocol (`_get_enum_discriminant`), on the `calculate_volume` function.

| Workers | Encoding | Mojo 1.2 (ms) | New protocol (ms) | Throughput difference | 
| 1 | Tagless | 15.881 | **8.214** | +93.3% | 
| 1 | Tagged | 9.572 | 9.685 | -1.2% | 
| 10 | Tagless | 2.879 | **1.932** | +49.0% | 
| 10 | Tagged | 2.587 | 2.474 | +4.6% | 

 Tagless calculation throughput improves about **1.93× with one worker** and **1.49× with ten workers**.
