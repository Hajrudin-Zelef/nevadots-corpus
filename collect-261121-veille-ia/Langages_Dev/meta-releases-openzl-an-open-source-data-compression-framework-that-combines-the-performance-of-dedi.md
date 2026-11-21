---
id: collect-261121-veille-ia/langages-dev/meta-releases-openzl-an-open-source-data-compression-framework-that-combines-the-performance-of-dedi
title: "Meta releases OpenZL, an open-source data compression framework that combines the performance of dedicated tools with the ease of general-purpose tools."
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2025-10-06"]
keywords: ["compression", "openzl", "data", "tools", "framework", "meta", "performance", "dedicated"]
source: https://gigazine.net/gsc_news/en/20251007-meta-openzl-graph-based-model-compression
source_anchor: ""
source_lines: [1, 40]
section: "Langages_Dev"
---
# Meta releases OpenZL, an open-source data compression framework that combines the performance of dedicated tools with the ease of general-purpose tools.


 Meta has announced the **OpenZL** compression framework, which achieves high compression rates while maintaining high speed. By building dedicated compression programs optimized for specific formats, it is said to achieve a level of performance that cannot be achieved with general data compression tools.

 **[2510.03203] OpenZL: A Graph-Based Model for Compression**

 **https://arxiv.org/abs/2510.03203** 


 **OpenZL**

 **https://openzl.org/**

 **Introducing OpenZL: An Open Source Format-Aware Compression Framework - Engineering at Meta**

 **https://engineering.fb.com/2025/10/06/developer-tools/openzl-open-source-format-aware-compression-framework/**

 On October 6, 2025 local time, Meta announced the public release of a new data compression framework called 'OpenZL.' OpenZL provides lossless compression for structured data, achieving performance comparable to dedicated compression tools. It does this by applying a configurable series of transformations to input data, revealing hidden ordering within the data and making it easier to compress. Despite applying different combinations of transformations to each file type, all files can be decompressed using the same general-purpose tools.

 While typical data compression tools rely on a uniform processing strategy or waste cycles guessing which method to use, OpenZL saves cycles by making structures explicit input parameters, allowing compression to focus on a series of reversible steps that surface patterns before coding.

 The compression process begins with the user providing OpenZL with the shape of their data (either via a preset or a thin format description). An offline optimization component, the trainer, then builds an effective compression configuration that can be reused for similar data. At encoding time, this configuration is translated into a specific decoding recipe embedded in the frame. Finally, a universal decoder executes the recipe directly, without any out-of-band information.

 Below is an example of data compression using OpenZL. **SAO** (binary data containing the positions, proper motions, magnitudes, etc. of 258,996 stars), part of **the Silesia Compression Corpus** , is compressed using three data compression tools: OpenZL, **zstd-3** , and **xz-9** . The compression was performed on a Mac running an M1 processor. OpenZL achieves better compression ratios and faster compression speeds than typical lossless compression tools that only handle byte sequences.

| tool | zstd -3 | xz -9 | OpenZL | 
|---|---|---|---|
| Compressed Size | 5,531,935B | 4,414,351B | 3,516,649B | 
| Compression ratio | 1.31 times | 1.64 times | 2.06 times | 
| Compression Speed | 220MB/s | 3.5MB/s | 340MB/s | 
| Thawing speed | 850MB/s | 45MB/s | 1200MB/s | 

Importantly, OpenZL can achieve higher compression ratios while maintaining or improving speed, which is crucial for data center processing pipelines.

OpenZL is developed as open source and is available on GitHub.

**GitHub - facebook/openzl: A novel data compression framework**

**https://github.com/facebook/openzl**
