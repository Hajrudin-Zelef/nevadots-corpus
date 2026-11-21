---
id: collect-261121-veille-ia/docs-techniques/index
title: "Index"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["compression", "openzl", "graph", "data", "index", "structure", "been", "requires"]
source: http://openzl.org/blog/
source_anchor: ""
source_lines: [1, 7]
section: "Docs_Techniques"
---
Starting in v0.2.0, OpenZL ships its own native LZ engine, which can offer better performance than using Zstandard or LZ4. While OpenZL's main target is structured data, LZ is still a core backend compression technique used in nearly all compression graphs, applied after the higher-order structure has been removed. Additionally, there are use cases where the structure of the data is unknown, and LZ is a good default.

*Letting a neural network build the compression graph*

OpenZL compresses data by chaining processing layers in any order. This is powerful, but requires careful configuration: choosing the right combination of codecs for a given input has always been its greatest challenge. The Compression Transformer now makes that choice automatically, building the compression graph on the fly, one decision at a time. It needs no per-source training, no manual tuning, and no change on the decompression side. It is available from both the API and the CLI.

OpenZL’s graph model is key to its ability to outperform generic compressors. Understanding and optimizing OpenZL compression requires understanding the compression graph. As part of the team’s continued focus on the user experience, we’ve developed an interactive visualization tool that makes this task simple.
