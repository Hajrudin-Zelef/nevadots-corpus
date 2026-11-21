---
id: collect-261121-veille-ia/langages-dev/meta-opens-up-openzl-as-format-aware-compression-framework
title: "Meta Opens Up OpenZL As Format-Aware Compression Framework"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["openzl", "compression", "format-aware", "framework", "meta", "opens", "high", "data"]
source: https://www.phoronix.com/news/Mesa-OpenZL-Compression
source_anchor: ""
source_lines: [1, 17]
section: "Langages_Dev"
---
# Meta Opens Up OpenZL As Format-Aware Compression Framework

OpenZL aims to provide high compression ratios while maintaining high speed. High compression and high performance typically don't go hand-in-hand for generic compressors while OpenZL is format-aware in trying to leverage the best of both worlds using a specialized compressor depending upon the format.

OpenZL focuses on lossless compression for structured data and the performance of format-specific compressors but with just a single executable binary.

They explained in their announcement that OpenZL comes after hitting rather a wall in Zstandard development:

"However, while it was improved over time, remaining within the Zstandard framework offers diminishing returns. So we started looking for the next great leap in data compression.


In this quest, one pattern kept repeating: Using generic methods on structured data leaves compression gains on the table. Data isn’t just byte soup. It can be columnar, encode enums, be restricted to specific ranges, or carry highly repetitive fields. More importantly, it has predictable shapes. A bespoke compressor that leans into that structure can beat general-purpose tools on both ratio and speed. But there’s a catch — every bespoke scheme means another compressor and decompressor to create, ship, audit, patch, and trust.


OpenZL is our answer to the tension between the performance of format-specific compressors and the maintenance simplicity of a single executable binary."

Those wishing to learn more about OpenZL can do so via this Facebook Engineering blog post. The OpenZL code is BSD licensed and is available from GitHub.
