---
id: collect-261121-veille-ia/langages-dev/simdjson-simdjson-json-is-everywhere-on-the-internet
title: "simdjson/simdjson: JSON is everywhere on the Internet…"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simdjson", "json", "parsing", "parser", "everywhere", "internet", "library", "twitter"]
source: https://www.bestgithubrepositories.com/simdjson/simdjson
source_anchor: ""
source_lines: [1, 172]
section: "Langages_Dev"
---
## simdjson : Parsing gigabytes of JSON per second

JSON is everywhere on the Internet. Servers spend a *lot* of time parsing it. We need a fresh
approach. The simdjson library uses commonly available SIMD instructions and microparallel algorithms
to parse JSON 4x  faster than RapidJSON and 25x faster than JSON for Modern C++.

- **Fast:** Over 4x faster than commonly used production-grade JSON parsers.
- **Record Breaking Features:** Minify JSON  at 6 GB/s, validate UTF-8  at 13 GB/s,  NDJSON at 3.5 GB/s.
- **Easy:** First-class, easy to use and carefully documented APIs.
- **Strict:** Full JSON and UTF-8 validation, lossless parsing. Performance with no compromises.
- **Automatic:** Selects a CPU-tailored parser at runtime. No configuration needed.
- **Reliable:** From memory allocation to error handling, simdjson's design avoids surprises.
- **Peer Reviewed:** Our research appears in venues like VLDB Journal, Software: Practice and Experience.

### Table of Contents

### Real-world usage

If you are planning to use simdjson in a product, please work from one of our releases.

### Quick Start

The simdjson library is easily consumable with a single .h and .cpp file.

1. 
Prerequisites: `g++` (version 7 or better) or`clang++` (version 6 or better), and a 64-bit
system with a command-line shell (e.g., Linux, macOS, freeBSD). We also support programming
environments like Visual Studio and Xcode, but different steps are needed. Users of clang++ may need to specify the C++ version (e.g.,`c++ -std=c++17` ) since clang++ tends to default on C++98.
2. 
Pull simdjson.h and simdjson.cpp into a directory, along with the sample file twitter.json. You can download them with the `wget` utility:```
wget https://raw.githubusercontent.com/simdjson/simdjson/master/singleheader/simdjson.h https://raw.githubusercontent.com/simdjson/simdjson/master/singleheader/simdjson.cpp https://raw.githubusercontent.com/simdjson/simdjson/master/jsonexamples/twitter.json
```
3. 
Create `quickstart.cpp` :

```
#include <iostream>
#include "simdjson.h"
using namespace simdjson;
int main(void) {
    ondemand::parser parser;
    padded_string json = padded_string::load("twitter.json");
    ondemand::document tweets = parser.iterate(json);
    std::cout << uint64_t(tweets["search_metadata"]["count"]) << " results." << std::endl;
}
```
1. `c++ -o quickstart quickstart.cpp simdjson.cpp`
2. `./quickstart`

```
 100 results.
```
### Documentation

Usage documentation is available:

- Basics is an overview of how to use simdjson and its APIs.
- Builder is an overview of how to efficiently write JSON strings using simdjson.
- Performance shows some more advanced scenarios and how to tune for them.
- Implementation Selection describes runtime CPU detection and how you can work with it.
- API contains the automatically generated API documentation.
- Compile-Time Parsing presents our compile-time parsing function (C++26 only).

### Godbolt

Some users may want to browse code along with the compiled assembly. The following examples use simdjson 5.0.1:

- C++26 reflection example
- simdjson examples with errors handled through exceptions
- simdjson examples with errors without exceptions

### Performance results

The simdjson library uses three-quarters less instructions than state-of-the-art parser RapidJSON. To our knowledge, simdjson is the first fully-validating JSON parser to run at gigabytes per second (GB/s) on commodity processors. It can parse millions of JSON documents per second on a single core.

The following figures show how fast simdjson maps JSON to and from your own C++ structs, using C++26 static reflection with GCC 16.2 (-O3). The task is to parse twitter.json into native data structures and to serialize them back to JSON, on an Intel Xeon Gold 6548N (Emerald Rapids) processor. We compare against the fastest C, C++ and Rust libraries, including Glaze and Serde. The simdjson library offers full unicode (UTF-8) validation and exact number parsing. You can reproduce these results with our static reflection benchmarks.

The simdjson library offers high speed whether it processes tiny files (e.g., 300 bytes) or larger files (e.g., 3MB).

For NDJSON files, we can exceed 3 GB/s with our multithreaded parsing functions.

### Packages

### Bindings and Ports of simdjson

We distinguish between "bindings" (which just wrap the C++ code) and a port to another programming language (which reimplements everything).

- ZippyJSON: Swift bindings for the simdjson project.
- libpy_simdjson: high-speed Python bindings for simdjson using libpy.
- pysimdjson: Python bindings for the simdjson project.
- cysimdjson: high-speed Python bindings for the simdjson project.
- fastpysimdjson: fast JSON parsing in Python using simdjson.
- simdjson-rs: Rust port.
- simdjson-rust: Rust wrapper (bindings).
- SimdJsonSharp: C# version for .NET Core (bindings and full port).
- SimdJson.Net: .NET bindings for the On-Demand API, shipped as a NuGet package with prebuilt native libraries.
- simdjson_nodejs: Node.js bindings for the simdjson project.
- simdjson_php: PHP bindings for the simdjson project.
- simdjson_ruby: Ruby bindings for the simdjson project.
- fast_jsonparser: Ruby bindings for the simdjson project.
- simdjson-go: Go port using Golang assembly.
- rcppsimdjson: R bindings.
- simdjson_erlang: erlang bindings.
- simdjsone: erlang bindings.
- lua-simdjson: lua bindings.
- hermes-json: haskell bindings.
- zimdjson: Zig port.
- simdjzon: Zig port.
- JSON-Simd: Raku bindings.
- JSON::SIMD: Perl bindings; fully-featured JSON module that uses simdjson for decoding.
- gemmaJSON: Nim JSON parser based on simdjson bindings.
- simdjson-java: Java port.
- mruby-fast-json: mruby binding with high API coverage.
- simdjson-dart: Dart bindings for the simdjson project.

### About simdjson

The simdjson library takes advantage of modern microarchitectures, parallelizing with SIMD vector instructions, reducing branch misprediction, and reducing data dependency to take advantage of each CPU's multiple execution cores.

Our default front-end is called On-Demand, and we wrote a paper about it:

- John Keiser, Daniel Lemire, On-Demand JSON: A Better Way to Parse Documents?, Software: Practice and Experience 54 (6), 2024.

Some people enjoy reading the first (2019) simdjson paper: A description of the design and implementation of simdjson is in our research article:

- Geoff Langdale, Daniel Lemire, Parsing Gigabytes of JSON per Second, VLDB Journal 28 (6), 2019.

We have an in-depth paper focused on the UTF-8 validation:

- John Keiser, Daniel Lemire, Validating UTF-8 In Less Than One Instruction Per Byte, Software: Practice & Experience 51 (5), 2021.

We also have an informal blog post providing some background and context.

For the video inclined, we had a talk at QCon San Francisco 2019


(It was the best voted talk, we're kinda proud of it.)

We also had a CppCon 2025 talk. We show how C++26 reflection allows for one-line serialization (to_json(player)) or deserialization—without invasive macros or manual mapping—using nothing but the C++ standard library. Whether you’re a performance junkie or simply interested in the roadmap for the next decade of C++ development, watch our full talk!

### Citing this work

If you use simdjson in published research, please cite the software library. A suitable BibTeX entry is:

```
@misc{simdjson,
  title={{The simdjson library: Parsing Gigabytes of JSON per Second}},
  author={Daniel Lemire and Geoff Langdale and John Keiser and Paul Dreik and Francisco Thiesen and others},
  year={2019},
  howpublished={Software library},
  note={https://github.com/simdjson/simdjson}
}
```
### Funding

The work is supported by the Natural Sciences and Engineering Research Council of Canada under grants RGPIN-2017-03910 and RGPIN-2024-03787.

### Contributing to simdjson

Head over to CONTRIBUTING.md for information on contributing to simdjson, and HACKING.md for information on source, building, and architecture/design.

### License

This code is made available under the Apache License 2.0 as well as under the MIT License. As a user, you can pick the license you prefer.

Under Windows, we build some tools using the windows/dirent_portable.h file (which is outside our library code): it is under the liberal (business-friendly) MIT license.

For compilers that do not support C++17, we bundle the string-view library which is published under the Boost license. Like the Apache license, the Boost license is a permissive license allowing commercial redistribution.

For efficient number serialization, we bundle Junekey Jeon's implementation of the Dragonbox algorithm for binary to decimal floating-point numbers (https://github.com/jk-jeon/dragonbox). The Dragonbox implementation is provided under the Apache License Version 2.0 with LLVM Exceptions (LICENSE-Apache2-LLVM or https://llvm.org/foundation/relicensing/LICENSE.txt) or the Boost Software License Version 1.0 (LICENSE-Boost or https://www.boost.org/LICENSE_1_0.txt).

For runtime dispatching, we use some code from the PyTorch project licensed under 3-clause BSD.
