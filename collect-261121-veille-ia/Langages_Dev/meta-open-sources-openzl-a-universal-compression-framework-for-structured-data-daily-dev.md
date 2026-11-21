---
id: collect-261121-veille-ia/langages-dev/meta-open-sources-openzl-a-universal-compression-framework-for-structured-data-daily-dev
title: "Meta Open Sources OpenZL: A Universal Compression Framework for Structured Data | daily.dev"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["compression", "openzl", "data", "structured", "meta", "framework", "universal", "open"]
source: https://daily.dev/posts/meta-open-sources-openzl-a-universal-compression-framework-for-structured-data-1b2j4opoq
source_anchor: ""
source_lines: [1, 1]
section: "Langages_Dev"
---
Meta released OpenZL, an open-source compression framework optimized for structured data like Protocol Buffers, database tables, and ML tensors. Unlike general-purpose compressors that treat data as raw bytes, OpenZL explicitly models data structures to achieve better compression ratios and faster speeds than Zstandard. It features a universal decompressor that executes embedded compression plans, eliminating the need for external metadata and enabling fleet-wide updates from a single binary. Benchmarks show OpenZL achieves 1.64× compression ratio at 340 MB/s compression and 1200 MB/s decompression speeds on structured datasets, outperforming both zstd and xz while maintaining deterministic decompression behavior.
