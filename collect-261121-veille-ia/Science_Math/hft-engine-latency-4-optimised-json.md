---
id: collect-261121-veille-ia/science-math/hft-engine-latency-4-optimised-json
title: "HFT Engine Latency - 4: Optimised JSON"
domain: science-math
role: reference
task: article
actors: []
dates: []
keywords: ["json", "latency", "parser", "data", "allocations", "message", "memory", "market"]
source: https://automatedquant.substack.com/p/hft-engine-latency-part-4-optimised
source_anchor: ""
source_lines: [1, 85]
section: "Science_Math"
---
# Introduction

In the previous post on performance tuning of Apex, we identified message parsing as a major contributor to latency. Profiling revealed that the root cause lay in the JSON parsing library, which was found to perform memory allocations for every message processed.

This article continues that investigation. The aim is to replace the existing general-purpose parser with one designed for zero heap allocations. We will walk through the integration of this alternative parser, measure its impact on latency, examine what problems remain and draw some lessons it offers for building low-latency trading systems.

# Zero allocation JSON

Apex currently uses JSON for Modern C++ (also known as *nlohmann::json*).  This is a highly expressive, general-purpose JSON parser.  It was initially chosen for its ease of use and integration, primarily used for parsing configuration files, and then became the default choice for processing market data.

However, as noted in the previous post, when processing Binance market data it was never faster than 3.8 µs, likely due to the memory allocations it performs during parsing. While 3.8 µs may seem a short duration, this is actually a significant proportion of time for a low latency trading system that ideally should aim for a tick-to-trade latency budget of under 10 µs.

Since memory allocations contribute to latency, switching to a parser that performs zero allocations should, in theory, reduce latency. But how to achieve this? The options are either to build our own custom parser or adopt an alternative third-party library.

Building a high performance market data specific JSON parser is feasible.  Market data messages are *mostly* structured in a repetitive format, so one approach would be to assume that the locations of field values are mostly fixed.  The idea would be to maintain a list of byte offsets to fields of interest (message type, ticker, size, price, etc.), and use these for direct access - thereby removing the need to scan through the whole data.  This is illustrated below, showing the offsets into a Binance market data *aggTrade* message:

However, “mostly” is doing a lot of work here. In reality field offsets would shift by a few bytes if, for example, the ticker varies in length from one message to the next. Similarly, numeric values can vary in length, so also requiring offset adjustments. While this approach could work, the main drawback is the time and effort needed to craft optimised code and handle edge cases.

When experimenting in ways to reduce latency, speed of progress is valuable. We want to get feedback quickly, verify our approach, and move on to other problems once enough of a win is locked in. For this reason, rather than building a complete parser from scratch, we adopt an optimised external library: not only is this faster, but it also comes with a predictable sub-project completion time.

A couple of libraries offer zero memory allocation. The one that claims to be the fastest is simdjson, advertising “25x faster than JSON for Modern C++”. While this impressive claim is likely true only for highly specific benchmarks, our immediate goal is simply to eliminate memory allocations.

As the name suggests, simdjson uses SIMD instructions to increase data processing bandwidth by utilising SSE and AVX instructions. It also offers an on-demand approach, meaning it only parses the fields we care about, rather than constructing a full document model.

So rather than building our own JSON parser, Apex will be enhanced to use simdjson on the critical path (JSON for Modern C++ will still be used on the non-critical path). The move to a custom parser can be saved until later, and only if the likely performance benefits justify the time and effort required.

# Integrating simdjson

Integrating simdjson is relatively straightforward. The library is delivered as a pair of source files, which are added directly to the Apex source tree, included in the CMake configuration, and compiled alongside the existing code.

Additional compiler options are required to tune simdjson for use in a low-latency trading engine. In particular, we disable the generation of AVX instructions and explicitly enable SSE. AVX instructions can trigger temporary CPU down-clocking on some processors, which is undesirable in a HFT environment, where consistently low latency is more important than temporary peak throughput. Below are the compiler options added:

`-march=westmere -mno-avx -mno-avx2 -mno-avx512f`

When simdjson parses raw JSON, it requires the input data be padded with an additional 64 bytes. This padding allows for safe and efficient use of SIMD instructions. To accommodate this, the Apex WebSocket code was modified to append null padding to the end of every message emitted. While this change introduces a small amount of extra work, the latency cost is negligible compared to the savings that should be achieved by eliminating memory allocations.

Using the simdjson parser to extract field values efficiently requires some care. Best performance is achieved when fields are accessed in the order they appear in the raw JSON input. The parser is designed to prefer a single pass through the data, avoiding the need to re-parse fields multiple times. One effective pattern - used in Apex as shown next - is to iterate over each field of a JSON object in encountered order and use a switch statement to handle fields that are relevant. This approach differs from the previous parser, which allowed random access but at the cost of additional allocations and a document object model.

# Performance

With simdjson integrated the engine was re-run to collect performance data. As before, all measurements were taken on the same system: a 3.40 GHz Core i7, with C-states and P-states tuned for lowest latency (as described previously). The engine was run for several hours, collecting millions of data points to allow for stable metrics.

The following table summarises the resulting measurements. The P50 total tick-to-model latency is now 14.8 µs, down from 28.0 µs previously - a substantial improvement. The minimum total tick-to-model latency also dropped significantly, from 6.3 µs to 2.4 µs.

Focusing specifically on the message parsing stage (S4), the minimum latency is now 0.3 µs, compared to 3.8 µs with the previous parser. This reduction is a direct consequence of replacing the allocation heavy DOM-style parser with simdjson’s on-demand, zero-allocation approach. Even at the P99 level, the improvement remains pronounced: parsing now takes 1.3 µs, versus 9.2 µs previously. This range, 0.3 to 1.3 µs, provides a useful upper bound on the additional latency that could be almost fully eliminated by moving to a binary protocol.

The next chart compares P50 latency across each inbound stage. As expected, the most significant improvement appears in S4 (message parsing). Notably, and perhaps unexpectedly, improvements are also observed in S1 (socket read) and S2 (SSL). The improvement in S3 (WebSocket) is expected, as this stage reflects queuing effects.

# Queuing effects

Queuing effects remain a significant factor in the raw measurements of the WebSocket stage (S3). Instrumentation within Apex captures timestamps at various inbound milestones. As a result, the recorded latency for an individual message includes not only the time duration being actively processed but also the time spent processing any preceding messages that arrived ahead of it.

The cause is several market data messages arriving together in a single network packet.  When this happens, and it occurs often, messages later in the batch will naturally exhibit higher elapsed times while they wait for earlier messages to be fully processed.  This is not “waiting” in the sense of blocking on a lock, but rather an artefact of the inbound thread having to loop over, and *fully* process, each message of the batch, one at a time, in the order they arrived off the socket.

To demonstrate this effect we can try to reduce batching and observe the impact on performance. A quick approach is to subscribe to a single, relatively low-traffic instrument. In the following experiment, the engine was configured to subscribe to a single name, ADAUSDT. This trades less often than BTCUSDT, and so will generate less market data events. With just a single instrument and a reduced update rate, queuing effects will be significantly diminished, allowing us to observe closer to the engine’s true processing latency.

The next table shows the resulting measurements, which are quite revealing. Up to the 75th percentile, messages are processed at around 10 µs, total tick-to-model. Only at P95 do queuing effects reappear. This gives a much clearer view of the engine’s intrinsic performance, mostly decoupled from queuing artefacts.

This experiment also highlights an important architectural consideration for HFT: it is essential to understand what an engine can achieve in its most optimised configuration. If the goal were to trade, for example, ten instruments concurrently, there are multiple deployment options. One approach is to trade all ten names from a single engine instance, accepting the higher latency that inevitably comes with increased message load and queuing. An alternative is to deploy multiple engine instances, potentially one per symbol, perhaps each with dedicated CPU resource. While the latter approach is operationally more challenging, it preserves low and predictable latency.

Such considerations are particularly important in low latency equity trading. One typical example is an index arbitrage strategy, where the single-name constituents of an index are traded simultaneously to the main index futures contract. For example, when trading the German DAX, such a strategy would need to concurrently trade up to 40 constituent stocks, executing each as rapidly as possible.

# Still some problems

The motivation for integrating simdjson into Apex was to reduce memory allocations when parsing market data. The performance improvements indicate that this was a worthwhile upgrade. However, it is useful to revisit our profiling tools to confirm whether any memory allocations remain on the critical path.

The tool *heaptrack* is again used to profile memory usage.  The following graph shows total memory allocations made for a short run of Apex.  Disappointingly we still see approximately 4,000 allocations per second.  While this is an improvement on the previous performance (which clocked-in at around 8,000 per second) it is still very poor and introduces unnecessary latency.

Heaptrack also allows us to drill down into the source of these allocations.  The top-down view shows that the vast majority arise inside *websocketpp*, a third party library used by Apex to handle the WebSocket protocol.  This is plausible: the library could be allocating memory to store completed messages before they are passed to the parsing stage.  The top-down view also confirms few allocations now arise during JSON parsing, which is as expected.

Apex’s inbuilt memory tracker confirms the same pattern.  The following output, which tracks heap allocations per scope, shows that two heap allocations per message are made inside the *hybi13::consume* function (part of websocketpp).

# Summary

This article has described the continued latency optimisation of the Apex trading engine. The primary goal was to replace the existing JSON parser with one that performs zero memory allocations. The new parser, simdjson, is a high-performance, SIMD based library that parses JSON on demand without allocating memory.

While some might question why JSON is used instead of a binary protocol (such as SBE), the answer is practical: not all exchanges provide SBE, so efficiently parsing JSON allows Apex to operate across all crypto exchanges. Additionally, JSON remains widely used on the order entry side, to manage orders and receive executions. Apex will support SBE in the future, but for now JSON remains a sensible choice due to its broad exchange coverage.

Performance improvements with simdjson were significant. The P50 tick-to-model latency was almost halved, from approximately 28 µs to 14 µs, while the minimum latency dropped from 6.3 µs to 2.4 µs. Improvements were also observed in other unrelated parts of the engine, demonstrating the broader impact of reducing heap allocations.

Despite these gains, allocations on the critical path remain a problem. The WebSocket library performs allocations per message, which likely contributes a few hundred nanoseconds of latency that could be avoided with further optimisation. Additionally, reading data from the socket has emerged as the new largest contributor to latency, with elapsed times exceeding 3 µs at the 25th percentile, indicating a priority area for investigation.

Finally, we examined queuing effects and their impact on measured latency. By experimenting with reduced message batching we observed a P75 latency of around 10 µs, revealing the engine’s intrinsic performance. This is a good result for an engine that has not yet undergone aggressive, system-wide optimisation.

Up to this point, efforts have focused primarily on removing obvious bottlenecks, such as heap allocations on the critical path and OS turning, rather than exhaustive or marginal optimisation. There remains significant potential for further latency reduction, including the use of binary protocols. The current results represent still early steps in Apex’s ongoing latency journey.
