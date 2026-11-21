---
id: collect-261121-veille-ia/langages-dev/meta-s-openzl-0-3-decompresses-144-faster-than-zstd-and-hands-codec-picking-to-a-transformer-hardwar
title: "Meta's OpenZL 0.3 Decompresses 144% Faster Than Zstd and Hands Codec Picking to a Transformer - Hardware Busters"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["zstd", "meta", "openzl", "transformer", "numeric", "data", "decompresses", "compression"]
source: https://hwbusters.com/news/metas-openzl-0-3-decompresses-144-faster-than-zstd-and-hands-codec-picking-to-a-transformer/
source_anchor: ""
source_lines: [1, 24]
section: "Langages_Dev"
---
**The LZ engine gains another 33% on the decode side, while a neural selector beats zstd -19 by 35% on numeric data without any per-dataset training.**


Meta has released OpenZL 0.3, and the headline number is aimed squarely at the compressor its own engineers also maintain. On the Silesia corpus, the OpenZL 0.3 native LZ engine decompresses at 3,062 MB/s against 1,254 MB/s for Zstandard, while landing on essentially the same compression ratio (2.73 versus 2.74). That is the “144% faster than Zstandard” figure in the v0.3.0 release notes, and it is a 33% jump over OpenZL 0.2.0, which managed 2,288 MB/s back in May.

Compression speed barely moved: 467 MB/s now, 466 MB/s in 0.2.0, and 419 MB/s for Zstd. This release is almost entirely about the read path, which is where most compressed data spends its life. Logs, telemetry, game assets and package archives get written once and read over and over.

## Where the speed came from

Most of the decode gain comes from a new Huffman layout called PivCo, contributed by Marcin Żukowski, which is arranged so SIMD units can chew through it instead of walking the bitstream one symbol at a time. Meta also added NEON and SVE2 kernels with CI coverage, so Arm servers and laptops are no longer an afterthought. The LZ engine now spans the equivalent of Zstd levels 1 to 7, the window grows to 256 MiB, and a new `zli train --profile lz` command tunes the LZ parameters to a specific dataset.

## A transformer that builds the compressor

The more unusual addition is what Meta calls the Compression Transformer. OpenZL’s pitch since its debut last October has been that it understands data formats and assembles a graph of transforms and codecs to suit them, rather than treating everything as a flat byte stream. Until now, the best graphs came from training on samples of your own data. The transformer is a neural selector that builds those graphs on the fly for numeric streams, with no per-source training at all.

Meta tested it on 868 families of numeric streams: 34,737 files totalling 17.9 GB. Overall, the transformer reached a 7.76x ratio, against 5.75x for zstd -19 and 5.92x for xz -9, which works out to 34.9% better than Zstd’s slow mode. The advantage grows with integer width, from a modest 1.3% on 8-bit data to 24.5% on 16-bit, 39.2% on 32-bit and 85.2% on 64-bit values. Meta says the results land within 1.1% of graphs trained on matched data. That is the part that makes it practical, because nobody wants to train a compressor for every new table schema.

Smaller additions include a sparse_num codec for zero-heavy numeric streams, Zstd dictionary support, big-endian integer profiles, and a binary that is roughly 447 KiB lighter after a legacy model was removed.

## The fine print

A few caveats before anyone rips Zstd out of a pipeline. The LZ comparison is against Zstd’s fastest setting: the 0.2.0 notes spell out level 1 with a 64K window, and the identical Zstd figures in 0.3.0 point to the same configuration. Meta does not list the test hardware either. And the 35% ratio advantage applies to numeric streams, exactly where format-aware compression should shine, not to general text or mixed binaries.

OpenZL is also still a 0.x project and behaves like one. The maximum frame format version jumps from 24 to 27, so anything that has to stay readable by 0.2.0 needs the format version pinned to 24. Serialized compressors that use the numeric graph require 0.3.0, and a couple of API functions were renamed. As Phoronix points out, the release lands just shy of a year after OpenZL was announced. For storage teams the decode numbers are the story; for everyone else, it is a sign that Zstd, after a decade as the sensible default, now has a serious challenger from inside the same company.
