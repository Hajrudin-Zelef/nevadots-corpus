---
id: collect-261121-veille-ia/langages-dev/openzl-using-graphs-to-compress-smaller-and-faster-2
title: "OpenZL: Using Graphs to Compress Smaller and Faster"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["compression", "openzl", "compressors", "data", "faster", "compress", "ratios", "using"]
source: https://arxiv.org/html/2605.09928v1
source_anchor: ""
source_lines: [1, 604]
section: "Langages_Dev"
---
# OpenZL: Using Graphs to Compress Smaller and Faster

###### Abstract

In the last few decades, research techniques have improved lossless compression ratios by significantly increasing processing time. However, these techniques have not gained popularity in industry because production systems require high throughput and low resource utilization. Instead, real world improvements in compression are increasingly realized by building application-specific compressors which can exploit knowledge about the structure and semantics of the data being compressed. Application-specific compressor systems outperform even the best generic compressors, but these techniques have severe drawbacks—they are inherently limited in applicability, are hard to develop, and are difficult to maintain and deploy.

In this work, we show that these challenges can be overcome with a new compression strategy. We propose the “graph model” of compression, a new theoretical framework for representing compression as a directed acyclic graph of modular codecs. OpenZL implements this framework and compresses data into a self-describing wire format, any configuration of which can be decompressed by a universal decoder. OpenZL’s design enables rapid development of application-specific compressors with minimal code; its universal decoder eliminates deployment lag; and its need for a limited set of standard components minimizes maintenance burden. Experimental results demonstrate that OpenZL achieves superior compression ratios and speeds compared to state-of-the-art general-purpose compressors on a variety of real-world datasets. Compared to ratio-focused deep-learning compressors, OpenZL is competitive on ratio while being many orders of magnitude faster. Internal deployments at Meta have also shown consistent improvements in size and/or speed, with development timelines reduced from months to days. OpenZL thus represents a significant advance in practical, scalable, and maintainable data compression for modern data-intensive applications.

###### Index Terms:

compression, data models, database management, heterogeneous databases, training
## I Introduction

Compression research over the last decade has largely focused on leveraging machine learning to improve compression ratios [1, 2, 3, 4, 5, 6]. This benefits scenarios where minimizing data size is critical but speed is less important [7, 5, 3, 8]. Deep-learning approaches compress benchmark datasets at significantly better ratios than traditional techniques but achieve throughput on the order of 1KB/s and often require heavy GPU resources [7, 4, 2, 8].

By contrast, production workloads must strike a balance between compressed size and processing time. For this reason, almost all popular compressors on the market use a variant of LZ77 [9], as its fast speeds and reasonable compression ratio makes it suitable for latency-sensitive, high-volume environments. Indeed, the last great leap in production-scale compression was Zstandard (Zstd) [10], which combines LZ77 with entropy coding, and whose typical usage compresses on the order of 100 MB/s and decompresses on the order of 1 GB/s. Many papers investigating compression for real-world applications say the quiet part out loud: deep-learning systems are too slow and require too many resources to be serious contenders in production, despite superior ratios [11, 12, 13, 14, 15].

As a consequence, domain-specific compressors are increasingly the tool of choice for data-intensive workflows. In fields like genomics [16, 17, 18, 19, 20, 21], computer graphics [22, 23, 24, 25], and AI models [26], tailored compression algorithms have pushed the state of the art in both academic and industrial applications.

Across disparate read/write patterns, data lifetime requirements, and data organization, there is a clear through-line: knowing *anything at all* about the data being compressed yields better and faster compression than even the best generic compressors. And furthermore, the more structure that can be exploited out of the data, the better performance will be on both axes.

This then begs the question: why aren’t application-specific compressors more common? In other words, if there’s a clear way to be both smaller and faster than generic compressors, *why is it so rare?*

1. (i) 
Upfront investment is intractable. Designing and implementing compression algorithms requires significant expertise and effort. Zstd, for instance, contains over 100,000 lines of code, representing years of development and hand-optimization from a well-funded team. Beyond this, application-specific compression algorithms requires expertise not only in compression techniques, but also in the problem domain.
2. (ii) 
Inflexibility of solutions. Most custom compressors are designed to optimize for type of data. Scale issues are immediate once you try to onboard datasets that require different compression techniques. Additionally, just supporting new wire formats of the same data often requires extensive refactoring or even a complete rebuild of the library.

These constraints prevent smaller teams from adopting custom compression solutions. For larger enterprises that can fund their way out of these challenges, a different set of constraints arise when custom solutions are deployed at scale.

1. 3. 
Iteration is difficult. Releasing a production library means freezing the wire format and publishing long-term support guidance. Often this inhibits the speed of new development, as backwards compatibility limits what you can ship. This reduces the efficacy of custom compression because not all gains can be realized.
2. 4. 
Deployment is slow. Updating from version to requires that all the data readers are rolled out to support version before any of the writers are allowed to write the new version. Thus, without guarantees on library freshness, updating the wire format becomes untenable. This makes custom compression unsuitable for whole swaths of applications, including mobile app development and IoT devices.
3. 5. 
High-cardinality applications are difficult to secure. For data warehouses with diverse customer needs, deploying custom codecs for each of your clients’ needs multiplies the amount of code that needs to be maintained and security-hardened.

In this paper, we show that these challenges can be overcome with a new way of thinking about compression. We introduce OpenZL, a general compression engine that uses a graph-structured compression model with a self-describing wire format and a universal decoder. Specifically, OpenZL breaks from the typical monolithic compressor architecture by decomposing a compression into a DAG of composable codecs.

### I-A The Graph Model of Compression

Our main theoretical contribution is the graph model of compression, defined formally in section III. In summary, we define a compression graph as a computational graph [27] where the nodes are codecs and edges represent data generated as output of one codec and used as input for another. Codecs are defined simply to be multivariate functions, so are allowed to have multiple inputs and multiple outputs. The graph model allows us to think about compressors as abstract transformation engines and enables new ways of approaching the task of compression.

The computational graph also means decompression is purely procedural. Apart from the compressed data itself, all you need is the graph to decode any valid compressed frame. The universality of the decoder is an important result of the graph model.

### I-B Overview of Results

This paper’s main result is demonstrating that OpenZL’s graph model is both easy to use and expressive enough to cover a wide range of applications. OpenZL is able to address several pain points of application-specific compressors in order to simplify their development:

1. (i) 
Upfront investment is minimal. Compared to existing monolithic compressor architectures, OpenZL’s modular structure solves the flexibility problem and allows the same developer to quickly stand up compressors for many different formats. We show in section VI-C that little, if any, production code needs to be written to support new datasets.
2. (ii) 
Solutions are flexible. The composable graph model also means diverse datasets can be supported by simply creating new graphs. Our benchmark experiments in section VII demonstrate that many disparate data formats can be easily parsed to take advantage of OpenZL.

OpenZL is also an enterprise-scale solution for custom compression.

1. 3. 
Iteration is easy. The self-describing wire format means you can evolve a compressor’s graph over time without needing to make any changes to the decoder.
2. 4. 
Deployment is fast. A universal decoder elides the rollout calculation. The same core library can decode any graph given to it.
3. 5. 
High-cardinality applications are easy to secure. By centralizing compression onto one library, the maintenance and security surface area does not increase as new use cases are onboarded.

### I-C Paper Organization

Section III develops the graph model of compression, the underlying theoretical framework for OpenZL.

Section IV provides a worked example, using graph model principles to build a compressor for an example dataset.

Section V explores OpenZL’s implementation of the graph model and design choices of the code that allows it to take the leap from theory to practice.

## II Related Work

It is well-known that a universal algorithm that always compresses every input does not exist. If some inputs are shortened, others are necessarily lengthened. In other words, lossless compression algorithms are injective, and for injective it holds that .

### II-A Entropy Coding

It follows that a valid strategy is to fill the space unevenly, assigning short codes to frequent events, while sacrificing rarer ones with longer codes. This is the core of entropy coders, which have been well studied for decades, and offer a predictable ceiling, named the Shannon limit [28].

Huffman remains widely used [29]; arithmetic coding approaches the entropy limit with different latency/state trade-offs [30]. The ANS family offers arithmetic-like compression at higher speed [31], with Zstd’s FSE [32] as a table-driven tANS example.

In practice, entropy coders are mature; current work centers on high-throughput, optimized implementations and careful quantization of probabilities to the coder’s precision.

### II-B Reversible Transforms

To reach high compression ratios, entropy alone is not enough. Instead of compressing raw input symbols, a more favorable strategy is to first convert the input into another representation, using some reversible operation.

Reversible transforms are simply transforms with an inverse operation. Under this definition, an infinite number of transformations match this criteria. For example, encryption is technically a reversible operation. But it wouldn’t help for data compression. So it matters to select the correct one for the current context.

A better example would be an increasing numeric series, which can be converted into a series of deltas. Not only will this mechanically reduce the range of represented values, making the signal easier to compress, but from the operation may emerge a regularity (for example, sequential values are transformed into a series of 1s) that will make the resulting signal even easier to compress.

Another example, the Burrows–Wheeler Transform (BWT) [33] clusters similar contexts so nearby symbols look alike. The produced stream has the same size and content as the ingested one, but ordered differently. It’s often followed by move-to-front (MTF) coding [34], which reorders symbols by recency index, thus exposing locality for entropy coding. We can also present Transpose, which reorders bytes according to their rank in a fixed-size multi-bytes field.

Reversible Transformations are beneficial when they help expose structure for downstream modeling. Some transforms may produce less data than they ingested. But that’s not a rule, and is not necessary for a transform to be useful.

### II-C Reductive Transforms

Reductive Transforms are also reversible, but their main contribution is to reduce the amount of data to process by further stages.

For example, Run-length encoding (RLE) [35] collapses symbol runs. The LZ77 family [36] replaces repeated substrings with backward references, while the LZ78 one [36] replaces them by an index in a dynamic dictionary. Another common strategy is dictionary substitution, where frequent substrings are factored into a static table and the input is rewritten as indices into that table.

In these cases, substantial reduction in data size is the intended effect, reflected in both final compression ratio and processing speed (having less data to process requires less computing time).

### II-D General-Purpose LZ

Faced with so many possibilities, the challenge is to select a proper set of transformation stages. Most compressors address this challenge by settling for a fixed set of transformation stages deemed "good enough" for general purpose use.

For scenarios with strict throughput requirements, LZ4 [37] and Snappy [38] apply LZ-style parsing with a lightweight tagged format (varint lengths/distances) rather than a full entropy-coding stage. They are effective for structured and textual data where modest ratios suffice but (de-)compression speed is critical.

DEFLATE/gzip [39, 40] encodes literal/length and distance symbols, intertwined in a single Huffman-coded stream, using (static or dynamic) Huffman trees. Brotli [41] improves on gzip by using context models for literals and offsets. Zstd [42] factors tokens into four logical streams (literals, literal-lengths, match-lengths, offsets) and uses either Huffman or FSE depending on mode. LZMA (as used in xz) combines LZ parsing and a range coder with context models (it is not a “pure LZ” design, making it more powerful but also markedly slower) [43].

### II-E Prediction-Centric Compressors

Higher compression ratios hinge on accurate symbol prediction. PPM conditions on preceding contexts [44]; DMC learns an adaptive automaton [45]; context mixing (e.g., PAQ [46], cmix [2]) blends multiple predictors using ideas related to boosting. Today these are increasingly neural-net based. NNCP is a more recent approach in this vein [3]. Despite excellent ratios, these algorithms remain orders of magnitude slower (KB/s scale) and sequential, making them less attractive for datacenter hot paths than the much faster LZ options.

### II-F A Programmable Composition of Processing Stages

Staged pipeline designs are explicit in several widely used formats. PNG [47] applies a per-scanline prediction filter (None/Sub/Up/Average/Paeth) before DEFLATE, turning image structure into locally predictable residuals that the downstream coder compresses well; the filter choice is itself a stage decision recorded per row. Blosc [48] composes blocking, a shuffle/bitshuffle transform, and then a configurable backend codec (LZ4, Zstd, etc.) Parquet [49] composes per-column encodings (dictionary, delta, RLE/bit-packing) with a backend codec. In both of these cases, the composition is tunable. Indeed, Blosc offers BTune [50], which explores automatic choice/parameter search across Blosc2 codecs and filters. ZPAQ goes further: the archive stores a virtual-machine program [51] specifying contexts/transforms and the coder. Here, the pipeline is part of the compressed frame.

In practice, these designs are constrained by enumerated stage catalogs and fixed rules on how these stages can be composed. Even where plugins exist, tuning often focuses on parameters for individual stages rather than exploring different stage orderings or richer graphs. As a result, a large fraction of the transform design space—and the cross-stage interactions that dictate effectiveness—remains unexplored.

OpenZL represents the natural evolution of the staged pipeline design. Rather than limit the system to a strict linear flow, we introduce the graph model of compression.

## III The Graph Model of Compression

In this section we develop the abstract graph model. In the next section, we build a sample compressor using the graph model.

### III-A Data and Messages

In typical data compression parlance, a message is a sequence of bytes. In the graph model, we adopt a stricter requirement for messages.

###### Definition III.1 (Message Sets).

A message set is a non-empty subset of the universe of bitstrings.

Under this framing, a message is an element drawn from a message set. Rather than be any random bitstring, we impose semantic requirements on messages by restricting the possibility set. For instance, components may require that messages represent 64-bit integer arrays by requiring that all messages have bit-length divisible by 64. More restrictive representational requirements can also be imposed. For instance, sorted runs of bytes; or a particular semantic structure like a zip file.

### III-B Codecs

Fundamentally, a codec is little more than a function operating on message sets.

###### Definition III.2 (Codec).

An input (resp. output) is an ordered tuple of messages , each drawn from a potentially different message set . The input domain (resp. output domain) is the ordered tuple of these message sets, i.e. . A codec is a tuple of functions. The encoder is a mapping between a non-empty input domain and a non-empty output domain . The decoder maps to a possibly different regenerated domain . A codec is lossless if this mapping is invertible, that is, and

This definition intentionally de-emphasizes the inner workings of the codec. In this model, the input/output signature matters more than the exact implementation because the signature tells us how the information is transformed semantically. This is an important abstraction, as it enables us to consider composition at a higher level.

### III-C Composition and Graphs

In the graph model, compressors are graphs built from codec nodes. As a motivating example, consider the tokenize codec. Briefly, tokenize searches for repeated instances of the same “token”. It works by taking a message and outputting 2 messages: , the list of unique tokens in ; and , the “index” in of each token within .

Tokenization is sometimes an effective compressor on its own (such as when the message is composed of many repetitions of a few large tokens). More frequently though, its utility is as an intermediate transformation, which produces outputs better suited for subsequent processing. We can attach other codecs to each of the tokenize codec’s two outputs, which separately attack the problems of efficiently representing the contents of the tokens and the indices.

While the alphabet has the same type of content as the original message , the indices in are a sequence of integers rather than strings. While is a partial representation of , by transforming it into a fixed-width, integer sequence, we can bring techniques to bear on it that can’t be applied directly to . For instance, we may construct a compressor that sends to an LZ77 compressor and to an entropy encoder like Huffman. Figure 2 contains a visualization of this new compressor.

As the visualization implies, a compressor with multiple codecs conveniently organizes itself as a graph. Informally, a compression graph is a computational graph [27, 52] where the nodes represent codecs and edges represent input and output sets<sup>1</sup><sup>1</sup>
              1
              
              
              
              
              
              
              
            Technically, this is a *reversed* computational graph, since in typical depictions the feed-forward direction merges multiple inputs to produce the output, whereas a compression graph generates multiple outputs from the input. In particular, an edge between a parent and child codec indicates a sequential relationship, where one of the outputs of the parent is used as one of the inputs of the child.

###### Definition III.3 (Compression Graph).

Formally, a computational graph is a directed, acyclic, graph (DAG) where the nodes are functions and the edges represent function arguments (and data dependencies). A compression graph is a computational graph where each node is labelled with a codec , and edges are doubly-labelled with both an output from the source and an input to the target such that .

The sequence of transforms permitted by this model allows us to build compressors that exploit the semantics of the data much better than generic compressors. Semantic specialization in intermediate streams increases as you traverse the compression graph, increasing the efficiency of subsequent codecs. For entropy coders, such specialization can result in intermediate representations with lower entropy and thus a more compact code.

### III-D Universal Decoder

The compression graph inherits some useful properties from computational graphs. Notably, computational graphs are DAGs. And since every DAG admits a topological sort, this ensures that a well-defined compression graph always admits a valid feed-forward computation order (for compression) and a valid backpropagation order (for decompression).

Decompression thus operates by procedurally chaining together codec decoders in the order dictated by the topological sort. Moreover, since the proper decode procedure is uniquely determined by a combination of the final outputs and the graph structure, a graph-based compressor can exploit this property to provide a universal decoder, assuming it can decode all the codecs being used.

### III-E Compression Dynamicity

As presented, the graph model does not allow for any dynamism in the choice of codecs to run in a graph. However, strong guarantees on decodability motivates us to add flexibility with a small extension to the model.

###### Definition III.4 (Resolved Graph).

A resolved graph is a compression graph that contains only codecs.

###### Definition III.5 (Function graph).

Let denote the set of compression graphs. A function graph is a function . A dynamic compression graph is a compression graph where the nodes are either codecs or function graphs.

Function graphs can be described as “selectors”, choosing a graph based on the input, which itself may contain additional function graphs. At compression time, this expansion naturally modifies the graph being run. Figure 3 illustrates this process.

Readers familiar with lambda calculus may draw a vague parallel between function graph expansion and beta-reduction. Like beta-reduction, the function graph expansion process creates another valid graph, which may have more opportunities for function graph expansion. The “beta-normal form” for function graphs is the resolved graph, wherein no more expansion is possible.

A compression that succeeds will always generate a resolved graph. Since the resolved graph contains only regular codecs, it also completely specifies how to reconstruct the original input. In the graph model, the decoder cannot make any runtime decisions based on data presented, so the guaranteed existence of a resolved graph allows us to safely incorporate dynamism into OpenZL.

## IV Compressor generation by example

The freedom of composition allowed by the graph model can be daunting. Faced with limitless ways to interpret data, it matters to introduce methodologies to structure the effort. We present the following method:

This pattern emerges because the components of OpenZL that are actually good at compressing data—its suite of reversible/reductive transforms—work best on homogeneous streams of data. For inputs that aren’t already organized that way, those backend components require a frontend to parse and group the input into streams that the backend can then compress effectively.

To illustrate this pattern, we will use SAO, a part of the Silesia Compression Corpus [53]. This file follows a well-documented format [54] featuring a small header followed by an array of multi-fields records, each one describing a star.

#### IV-1 Frontend

The parser takes the input stream and separates the data into its logical components. For SAO, each record contains 6 fields. The parser turns the array of records into 6 arrays, each containing the concatenated data from the respective field (pictured in fig. 5). Including the header, which is passed as-is, the parser produces 7 total streams.

After parsing, grouping of fields is sometimes necessary to take full advantage of cross-field correlation. This involves partitioning the parsed streams and merging each partition into a single stream for downstream processing. The simplest (but by no means only) way to do this is to concatenate all streams in a partition. This is overkill for SAO but we use this technique extensively in the experiments (section VI-C).

#### IV-2 Backend

Now that each stream contains homogeneous data, we can focus on selecting a dedicated compression strategy for each stream. This means selecting a series of reversible/reductive codecs to maximize compression within a given speed budget. This stage is where compression expertise is particularly valuable.

In the case of SAO, we can manually take decisions based on visible characteristics:

- • 
SRA0 is a position on the X axis. Due to the way the table is generated, the index is mostly sorted, inviting the use of delta to reduce the range of values represented. This mechanically reduces the entropy of the resulting stream, making it easier to compress.
- • 
SDEC0 is a position on the Y axis. It’s not sorted, unlike the X axis, but we can at least exploit the fact that it’s bounded between a minimum and a maximum. This makes the higher bytes predictable. This can be exploited for better compression with the transpose operation.
- • 
The other fields (IS, MAG, XRPM, XDPM) share a common property: their cardinality is much smaller than their quantities, and there is no correlation between consecutive values. This makes them a good target for tokenize, described earlier.
- • 
The resulting dictionaries and index lists have very different characteristics. Both are numeric, but one is sparse, the other is dense and bounded. Therefore, they benefit from different compression strategies. They are pushed into different processing graphs.

As shown in table I, this simple analysis and manual decision is enough to produce a compressor<sup>2</sup><sup>2</sup>
              2
              
              
              
              
              
              
              
            The full implementation is at https://github.com/facebook/openzl/blob/icde26/cli/utils/compress_profiles.cpp#L25-L98 with both a higher compression ratio and faster speed than generic lossless compressors.

We present this manual example to intuitively illustrate how compression works and how graph choices materially impact compression performance. This could be pushed further—there are more complex relations in the data streams—but exploiting them increases graph complexity. Even with the framework provided by OpenZL, manually optimizing compressors requires substantial expertise. However, we demonstrate in section VI-C that this process can also be automated to yield good results, making OpenZL performance accessible to non-experts.

## V Implementation

We now turn from theory to briefly discuss OpenZL’s implementation of the graph model. This section provides an overview of the structure and design principles of the current open-source implementation, available at github.com/facebook/openzl.

### V-A Implementing the Graph Model

OpenZL first and foremost is an implementation of the graph model presented in section III. Much of the implementation is unremarkable from a theoretical perspective. We mention some notable exceptions here.

- Message Sets.
- 
OpenZL has a partial implementation of message sets. It would be unrealistic to allow specifying arbitrarily-specific sets, so we approximate it with a type system. There are currently 4 types: 
  - • 
bytes for opaque serial data.
  - • 
string for sequences of byte strings.
  - • 
struct(k) for fixed-size () records.
  - • 
numeric(w) a specialization of struct for host-endian 8, 16, 32, and 64-bit numbers.
- • 
- Codecs.
- 
Codecs are the lowest level in the OpenZL architecture. Each codec does one thing well. Codecs are split into two parts: encoder and decoder. Implementation-wise, each side is typically organized into two layers: a *kernel* and a*binding* . Kernels are small, deterministic, and allocation free; the binding around them handles types, bounds, and buffers. The vast majority of CPU time is spent in the kernel, so splitting in this way simplifies performance optimization work.
- Dynamism.
- 
We implement dynamism two ways. The function graph implements its conceptual namesake: these are regular codec encoders with the restriction that they cannot modify the input data, only call other codecs. The selector’s job is to output a graph based on input data. This provides a useful compromise between implementation power and abstract correctness.

### V-B The Software Stack

Novelties aside, OpenZL is fundamentally still an enterprise-scale compression library, so care was taken to design a product conducive to widespread deployment. The open-source OpenZL implementation is layered, with the goal of making the hot path efficient and verifiable. At the bottom sits the C11 core library, libopenzl, which exposes a stable surface and an execution engine for compression graphs. A thin C++ façade wraps that surface to provide RAII and strong typing. On top, a Python binding offers an API that integrates with data-science toolchains while preserving the core library’s determinism and memory discipline.

The choice of C11 for the core is deliberate: The wide portability and ubiquity of C toolchains, predictable memory semantics, universal ABI, and clear debuggability expectations are critical at this layer. By constraining the core to a narrow contract, higher layers can evolve at their own cadence without perturbing the on-wire format or the decoder.

### V-C Versioning and Decoding

In addition to software ergonomics, OpenZL addresses the problem of artifact compatibility within its design. For any continuously-deployed software ecosystem, it is inevitable that there are multiple versions of decoders are active at the same time. The traditional solution is to freeze the wire format so all decoders work off the same spec but this limits the evolution of the library.

OpenZL uses the concept of format versions to add flexibility. When a library version is released, it explicitly supports a range of format versions. At compression time, you select a format version that all your decoders support. Based on the selected version, the library will restrict the suite of functionality it can deploy during the compression. For instance, it can refuse to use new codecs that older formats do not support. In this way, the graph model naturally facilitates incremental binary evolution by breaking down wire-format evolution into a codec-by-codec process.

### V-D Iteration and Deployment

All OpenZL compressors are serializable using any number of graph representation schemes. These serialized compressors are very compact; for instance, the SAO example in the previous section serializes to <2KB. OpenZL is able to parse (and then compress) with these serialized compressors, so serialized compressors can be passed around and deployed like regular config files.

## VI Experimental Setup

In the next two sections, we demonstrate the flexibility and efficacy of OpenZL by building competitive compressors for a variety of datasets and data formats.

### VI-A Datasets

We evaluated the compression ratio of OpenZL on a number of publicly available datasets. Procedurally, we chose datasets with a variety of file formats. Table II summarizes these datasets.

#### VI-A1 Binance

An important resource in quantitative finance is candlestick data, which describe how the price of an asset changes over a given timeframe. The Binance dataset [55] is a collection of 1-minute candlestick data for the top 1000 cryptocurrency trading pairs on binance.com, as retrieved from Binance’s official API endpoint for historical candlestick data.

For our benchmark, we selected 15 Bitcoin candlestick records from the dataset. We convert these records to a “canonical” Parquet format, with no compression and default encoding.

#### VI-A2 NYC Taxi Trip Records

The New York City Taxi and Limousine Commission (TLC) is the agency responsible for licensing and regulating New York City’s taxis, for-hire vehicles, commuter vans, and paratransit vehicles. The TLC collects and publishes trip record information for each taxi and for-hire vehicle trip completed [56].

For our benchmark, we use Yellow and Green Taxi trip data from Q1 2025 (Jan–Mar). We convert these records to a “canonical” Parquet format, with no compression and default encoding.

#### VI-A3 Climate Reanalysis

The climate reanalysis is an important tool in climate study. The European Centre for Medium-Range Weather Forecasts (ECMWF) currently maintains ERA5, the fifth-generation of their global reanalysis dataset [57]. For our benchmark, we used 5 datasets from October 1987: 10m u-component of wind (`ERA5_wind`), mean sea level pressure (`ERA5_pressure`), snow density (`ERA5_snow`), downward UV radiation at the surface (`ERA5_flux`), and total precipitation (`ERA5_precip`). For each dataset, there are 720 hourly snapshots.

#### VI-A4 US Census

The anonymized 2020 US Census data is available via the Privacy-Protected Microdata File (PPMF) [58]. The PPMF data are organized into two huge CSV files containing household data (ppmf_unit) and people data (ppmf_person). We preprocessed the files by breaking them into 100 MB chunks, splitting between line breaks. This generates 548 ppmf_unit files and 1,256 ppmf_person files.

The Census Bureau also collects the yearly American Community Survey (ACS). The data are available via the Public Use Microdata Sample (PUMS) [59]. We build our corpus from the 5-year PUMS data from 2023 [59]. There are two sets of CSV files, one for people and one for households. We refer to these datasets as psam_p and psam_h.

### VI-B Hardware and Software

All benchmarks were run on a Lenovo P620 desktop with an AMD Ryzen Threadripper PRO 3995WX CPU, with 256 GB of memory (832 GB DDR4 3200MHz RDIMM ECC memory), and a 2 TB Samsung PM981a SSD. For a fair comparison, swap was not used. All benchmarks operate single-threaded in memory. Precision Boost was disabled for consistent speed numbers.

We compiled OpenZL with GCC 14 on Fedora Linux 41. Each dataset is also benchmarked against a list of widely-used traditional compressors and some modern deep-learning systems. We chose XZ, Zstd, and gzip/zlib as representative traditional compressors; we chose cmix and NNCP to represent modern deep-learning systems. For a fair speed comparison we did not allow GPU acceleration, which limited our choice of ML-based compressors to those that supported CPU computation.

Among the traditional compressors, XZ is well-regarded for its compression ratio and Zstd for its speed. Cmix is long-known for being the high watermark for compression ratio, often at the top of generic compression benchmarks.

### VI-C Compressor Generation and Training

For each dataset, we built OpenZL compressors following the structure described in the SAO example in Section IV: Parse into homogeneous streams, cluster similar streams, and compress the clusters. The parsers for CSV and Parquet were written manually (the GRIB data are just numeric arrays, so no parsing was needed). Diverging from SAO, we decided to automate the clustering and backend graph generation processes using a custom-purpose training script <sup>3</sup><sup>3</sup>
              3
              
              
              
              
              
              
              
            The automated trainer is open-source and accessible by calling zli train. Usage details can be found in the online quick-start guide..

The training script handles clustering and backend graph generation in separate training stages. After parsing the input data into a set of streams, as in the SAO example, the clustering trainer groups similar streams. Initially, each stream is assigned to a different “cluster”. The trainer greedily combines pairs of clusters whose combined compressed size is smaller than the summed individual compressed sizes. It then repeats the process until it reaches a local minimum.

The backend graph generator uses the NSGA-II genetic algorithm [60] to build a Pareto-optimal set of compression graphs. Each compression graph is a DAG that can be manipulated by the algorithm. The population is seeded with a set of simple but commonly effective compression graphs. The crossover and mutation functions are taken from Genetic Programming [61], which is a natural fit because a compression graph is just a reversible computation graph.

Finally, the sets of Pareto-optimal backend graphs for each cluster are merged iteratively. Each set of graphs is merged into the accumulated Pareto-optimal set, then the merged set is pruned back down to entries by iteratively selecting the points with the highest crowding distance [60].

Table III gives some summary statistics on the training procedure for each test dataset. Anecdotally, we found that training on more data does not necessarily improve the performance of trained compressors. To use the SAO example again, we found that training on the first 1% of the data increases compression ratio by 29%—but training on the *entire* dataset increases compression ratio by only an additional 3%. This example suggests that performance plateaus quickly after building a representative sample. We hypothesize this is because semantic structure is high-signal so adding more training data only helps by preventing overfitting. For our experimental datasets, we used at least 3 files when training and aimed to use a test-train split of 99-1 for larger datasets.

## VII Experimental Results

Trained OpenZL compressors are able to beat competitors on at least one axis of performance, and sometimes all three. We start with section VII-A, conducting a pure compression ratio comparison without considering speed. This experiment showcases the theoretical high watermark of each compressor being tested. We follow this with an analysis of the speed vs. ratio tradeoff for compressors that have this configurability. Section VII-B does not compare NNCP or cmix, as they are not configurable.

### VII-A Best Compression Ratio

The results of the high-watermark experiment is presented in fig. 6. Since this experiment includes cmix and NNCP, we compared performance on excerpts of each dataset. Cmix especially is too slow to process a multi-gigabyte dataset in a reasonable timeframe. On all datasets, OpenZL compressors are able to exceed the compression ratios offered by traditional compressors and remain competitive with deep-learning compressors.

On numeric GRIB datasets, OpenZL performs well, beating NNCP on the majority of datasets and approaching cmix on as many. Unsurprisingly, there is a lot of structure in climate data that can be exploited to improve compression. Particularly, OpenZL’s ability to work with numeric data types sets it apart from traditional compressors, which must work byte by byte.

On the CSV datasets, OpenZL performs less well, and markedly worse than cmix. Recall from Section VI-C that for tabular formats, we trained compressors to cluster based on inter-column correlation. This limits our ability to exploit inter-row correlation and local correlations that don’t extend to the entire file. Nonetheless, the data show that inter-column correlation is still a powerful weapon; we still perform better than the traditional LZ-based compressors. Beyond this, CSV is fundamentally a plaintext format. This means we lose the edge from working with numeric fields in GRIB.

On Parquet datasets, we can combine our strengths of semantic understanding of integers with the column-based approach. OpenZL is able to capture the inter-column correlation by clustering similar columns, and is then able to build a specialized compression graph for each cluster of columns. Unsurprisingly, OpenZL achieves superior performance on these datasets, beating both NNCP and cmix.

While this experiment is focused on maximizing ratio, the full story would be incomplete without mentioning the difference in processing speeds between OpenZL and its competitors. Table IV summarizes these speed numbers. NNCP and cmix are irrecoverably slow, with both compression and decompression speeds 100,000 worse than OpenZL. In fact, OpenZL has the highest average compression speed out of *all* compressors, and is equivalent to XZ in decompression.

### VII-B Compressor Tradeoff Selection

OpenZL is not limited to pursuing aggressive compression ratios, and indeed neither are most production compressors. In addition to the ratio-focused compressors trained in the previous section, the custom trainer generates a Pareto-optimal frontier of compression graphs. We benchmarked these tradeoff points against the typical level system featured by other generic compressors. This experiment mirrors the analysis a production engineer would do when choosing a compressor to deploy. Often, speed is just as important as ratio; on compute-bound workloads, it may even be more important.

Every point on the plot represents a unique compression graph for OpenZL, or a unique compression level for other compressors. In many cases, the OpenZL tradeoff curve for ratio vs.  compression speed strictly dominates. In particular, on the Parquet and GRIB datasets, it would *never* be better to choose XZ or zlib, because there exists an OpenZL compressor that pareto-dominates it. However, for the CSV datasets OpenZL first has to parse the CSV file, which limits its speed.

Here, the power of the graph model is fully realized. Traditional compressors are limited to one pipeline of operations, or a small handful of predefined pipelines, so the performance tradeoffs they can offer are limited. Since OpenZL can create an entirely new compression graph for each point in its Pareto-optimal frontier, it is able to offer a significantly wider range of tradeoffs along all axes.

Of course, this power comes with a higher setup cost than a generic compressor, which only requires selecting a compression level. Without an effective parser and graph (automatically generated or otherwise), OpenZL won’t perform any better than traditional compressors.

## VIII OpenZL Deployment at Meta

Optimizing compression performance tradeoffs is important for enterprises operating at Meta’s scale. Prior to OpenZL, Zstd was the primary compressor used at Meta, since it offered Pareto-optimal performance across a wide variety of use cases [62]. This was the result of a more than decade-long drive for compression efficiency, achieved both by optimizing Zstd’s performance and converting uses of compression to Zstd. It eventually became clear that the headroom for further improvements from Zstd, within Zstd, or even with LZ compression in general, was fundamentally limited. Thus OpenZL was born.

OpenZL has now replaced a meaningful fraction of Zstd use in production at Meta. Much of the data at Meta is serialized using Thrift [63], both at rest and in transit. A custom parser that understands Thrift, in conjunction with training tools to specialize the compressor for individual use cases, allows OpenZL to effectively compress a wide range of traffic.

OpenZL integrations at Meta can be split into 3 broad and interconnected categories:

1. (i) 
Data warehouses: Data Warehouses store vast amounts of data for analytics and training, typically stored in a columnar databases. Compression in columnar databases is not a new technique; open-source formats like Parquet have already explored this to some success. However, the expressibility allowed by the graph model allows OpenZL to extract additional gains by better modeling the data.
2. (ii) 
Training data: Training data encompasses everything that models are trained on. It flows from the applications where it is logged, through the online and offline training pipelines, and to the training jobs. The same data is also stored and cached for inference. At each step along the way, the data is compressed to improve efficiency.
3. (iii) 
Model training: In addition to the training data, the models themselves are also compressed. During training, model checkpoints are saved frequently, so that training progress isn’t lost if the job fails. Then, when a new model is shipped, that checkpoint must be saved through the model lifetime. Compressing these models reduces storage, checkpoint overhead, and distribution bandwidth.

Table V provides a summary table of internal OpenZL deployments so far, with details for each service listed below.

- Nimble:
- 
Nimble [64] is a columnar database format used at Meta. Previously, Zstd was the only backend compressor offered by Nimble. Replacing Zstd with (untrained) OpenZL compressors immediately saved 10% compressed size compared to this baseline. Most of these gains came from labeling numeric data types as such, and stacks on top of the transformations that Nimble uses to pre-process its data <sup>4</sup><sup>4</sup> 4
                  
                  
                  
                  
                  
                  
                  
                Nimble applies transformations that improve query efficiency by operating on the encoded data, where OpenZL applies transformations that are useful for compression only.. Traditional compressors like Zstd compress bytes, so the columnar formats serialize numeric data to bytes before compressing, typically by Varint encoding. Since OpenZL operates directly on numeric data, it is able to skip this step, which improves query efficiency.
- Scribe:
- 
Scribe [65] is a log processing system used extensively at Meta. With training, OpenZL improved compression ratios by 15% compared to Zstd. This improvement is felt both in reduced storage costs and in higher network throughput. Increasing throughput has the additional benefit of improving training data quality since fewer records are dropped during traffic peaks. As noted in [65], Scribe data is constantly evolving. We’ve been able to maintain these ratio wins via regular training runs similar to the ones ran for the benchmark experiments.
- PyTorch model checkpoints:
- 
Model checkpoints are saved frequently during the training process. Ephemeral checkpoints are only stored for a short time, but anchor checkpoints are saved for the model’s lifetime. OpenZL leverages type information to save an average of 17% on storage for model checkpoints by compressing the floating point exponents, with savings varying based on the floating point data type. Checkpoint saving and loading is network bound, so the 17% checkpoint size reduction also reduces checkpoint overhead by 15% on training machines, which improves GPU utilization.
- Feature storage:
- 
This service caches features stored in Thrift format for training data generation. Similar to Scribe, OpenZL was deployed with training for Thrift data. However, the Feature storage team chose a different trade-off point on the speed-ratio curve. Switching to OpenZL reduced storage by 10% and CPU utilization by 5% compared to Zstd level 6. Moreover, this was done solely by reusing existing components already built for Scribe.
- Log aggregator:
- 
This service joins realtime logs from several data sources for training data generation. OpenZL was able to reduce compressed size by 18% compared to Zstd. Initially, OpenZL was configured to compress the Thrift data directly. But, due to the latency sensitive nature of the service, they migrated to directly compressing arrays of integers with OpenZL. This allows them to skip the Thrift serialization & deserialization stages.
- Embedding storage:
- 
Compressing bfloat16 embeddings serialized in PyTorch’s torch.save() format reduces compressed size by 30% (thus allowing us to store 43% more training data). Prior to OpenZL, compression was not deemed computationally profitable because traditional compressors struggle with floating-point data. Zstd, for instance, can’t shrink the data by more than 10%, even at the highest compression level. By generating a floating-point compressor, we were able to save storage while not regressing training performance. Beyond this, the development timeline for this new compressor was on the order of days due to reuse of existing components developed for PyTorch model checkpoints.

Overall, OpenZL has helped Meta bend the curve of AI growth. The compression improvements that OpenZL offers allow Meta to do more with the same amount of hardware—better training data compression means more data can be pushed through the same pipe; smaller data means less compute is spent reading data from the data warehouse; reduced network traffic for model checkpointing improves GPU utilization.

### VIII-A Training with Managed Compression

OpenZL’s modular treatment of the components of compression, and the development of tools that automate the configuration and composition of those components, mean that OpenZL lends itself well to offline training as described in Section VI-C. Although the configurations under consideration internally are different and more diverse, this training workflow nonetheless has the same overall shape as training a dictionary for Zstd.

And in fact, Meta’s Managed Compression system [66], which was originally designed to manage dictionaries for Zstd compression, has proven adept at training compressors for OpenZL. The trainer accepts a corpus of representative samples and an existing compressor; it can configure and parameterize nodes or even construct and replace sub-graphs throughout the compressor. After validation and benchmarking, the resulting compressor can then be re-serialized and deployed to the fleet.

This architecture has proven useful not only to find useful OpenZL configurations, but also to drive broad adoption of OpenZL at Meta: in the same way that this systematic approach to training made Zstd dictionary adoption frictionless, this infrastructure makes OpenZL easy to integrate, and thousands of trained OpenZL compressors are deployed to use cases at Meta through Managed Compression.

## IX Conclusions

In this paper, we propose a graph model of compression. This new theoretical model encourages composition of small and simple codecs. We demonstrate its effectiveness with OpenZL, a robust implementation of the graph model. The result is a scalable, production-ready system that offers unprecedented performance across diverse datasets. For a wide range of benchmark datasets, OpenZL is able to beat the best compression ratio offered by xz at an order of magnitude faster compression, despite needing to parse and understand the data. OpenZL demonstrates that the compression efficiency unlocked by understanding the data far outweighs the effort spent on parsing, and that this can be achieved via composition of a library of relatively simple codecs.

We hope that the positive results from Meta-internal use cases will motivate data owners to investigate their own wins from using OpenZL. In particular, we expect the automated training tools presented to be more than adequate to achieve compression wins that justify the resource investment in OpenZL.

### IX-A Future Work

The unreasonable effectiveness of our first foray into training leads us to believe that the graph model is uniquely positioned to facilitate ML-guided generation of compressors. We are tempted to view this as “the next big thing” in production-scale compression. Whereas compression research has up to now eluded those without domain expertise, we believe the future of application-specific compressors will be unlocked via investment in automated learning methods.

## Acknowledgement

We would like to thank Graham Cormode for his guidance through the publication process, especially with regard to narrative clarity and prioritization. We would like to thank Evan West for his input and advice on the text of the paper. We would like to thank our former interns Timothy Oei, Pedro Valero, Aryan Gandevia, and Faizaan Baig for their contributions to OpenZL. Finally, we would like to thank Aras Pranckevičius and Takayuki Matsuoka for beta-testing OpenZL before its open-source launch.

## References

- [1]
B. Knoll and N. d. Freitas, “A machine learning perspective on predictive
coding with paq8,” in *2012 Data Compression Conference* , April 2012,
pp. 377–386.
- [2] B. Knoll, “Cmix compressor,” https://www.byronknoll.com/cmix.html, 2014, accessed: 2025-04-30.
- [3] F. Bellard, “Lossless data compression with neural networks,” 2019. [Online]. Available: https://bellard.org/nncp/nncp.pdf
- [4]
M. Goyal, K. Tatwawadi, S. Chandak, and I. Ochoa, “Dzip: Improved
general-purpose lossless compression based on novel neural network
modeling,” in *2020 Data Compression Conference (DCC)* , March 2020, pp.
372–372.
- [5] B. Knoll, “lstm-compress,” https://github.com/byronknoll/lstm-compress, 2017.
- [6]
Y. Mao, Y. Cui, T.-W. Kuo, and C. J. Xue, “Trace: A fast transformer-based
general-purpose lossless compressor,” in *Proceedings of the ACM Web
Conference 2022* , ser. WWW ’22. New
York, NY, USA: Association for Computing Machinery, 2022, p. 1829–1838.
[Online]. Available: https://doi.org/10.1145/3485447.3511987
- [7]
A. Liu, S. Mandt, and G. V. den Broeck, “Lossless compression with
probabilistic circuits,” in *International Conference on Learning
Representations* , 2022. [Online]. Available:
https://openreview.net/forum?id=X_hByk2-5je
- [8] B. Zhang, D. Cheng, Y. Zhang, F. Liu, and W. Chen, “Compression for better: A general and stable lossless compression framework,” 2024. [Online]. Available: https://arxiv.org/abs/2412.06868
- [9]
A. Gupta, A. Bansal, and V. Khanduja, “Modern lossless compression techniques:
Review, comparison and analysis,” in *2017 Second International
Conference on Electrical, Computer and Communication Technologies
(ICECCT)* . IEEE, 2017, pp. 1–8.
- [10] Y. Collet, “Zstandard - fast real-time compression algorithm,” Facebook, Open source project, 2015. [Online]. Available: https://github.com/facebook/zstd
- [11]
R. Jumar, H. Maaß, and V. Hagenmeyer, “Comparison of lossless compression
schemes for high rate electrical grid time series for smart grid monitoring
and analysis,” *Computers & Electrical Engineering* , vol. 71, pp.
465–476, 2018. [Online]. Available:
https://www.sciencedirect.com/science/article/pii/S0045790617334791
- [12]
M. Thevenin, S. Pigoury, O. Thomine, and F. Gouillon, “A comparison of
lossless compression algorithms for altimeter data,” *EGUsphere* , vol.
2022, pp. 1–28, 2022. [Online]. Available:
https://egusphere.copernicus.org/preprints/2022/egusphere-2022-1094/
- [13]
K. Iqbal, N. Khan, and M. G. Martini, “Performance comparison of lossless
compression strategies for dynamic vision sensor data,” in *ICASSP 2020
- 2020 IEEE International Conference on Acoustics, Speech and Signal
Processing (ICASSP)* , May 2020, pp. 4427–4431.
- [14] Jan 2024. [Online]. Available: https://help.2brightsparks.com/support/solutions/articles/43000335985-comparison-of-compression-methods-and-levels
- [15] 2019. [Online]. Available: https://linuxreviews.org/Comparison_of_Compression_Algorithms
- [16]
G. E. Pibiri, “Sparse and skew hashing of k-mers,” *Bioinformatics* ,
vol. 38, no. Supplement_1, pp. i185–i194, 06 2022. [Online]. Available:
https://doi.org/10.1093/bioinformatics/btac245
- [17]
——, “On weighted k-mer dictionaries,” *Algorithms for Molecular
Biology* , vol. 18, no. 1, p. 3, 2023. [Online]. Available:
https://doi.org/10.1186/s13015-023-00226-2
- [18]
J. N. Alanko, E. Biagi, J. Mackenzie, and S. J. Puglisi, “Batched k-mer lookup
on the spectral burrows-wheeler transform,” in *2025 Proceedings of the
Symposium on Algorithm Engineering and Experiments (ALENEX)* , 2025, pp.
95–106. [Online]. Available:
https://epubs.siam.org/doi/abs/10.1137/1.9781611978339.8
- [19]
S. Chandak, K. Tatwawadi, I. Ochoa, M. Hernaez, and T. Weissman, “Spring: a
next-generation compressor for fastq data,” *Bioinformatics* , vol. 35,
no. 15, pp. 2674–2676, 12 2018. [Online]. Available:
https://doi.org/10.1093/bioinformatics/bty1015
- [20]
D. Lan, R. Tobler, Y. Souilmi, and B. Llamas, “Genozip: a universal extensible
genomic data compressor,” *Bioinformatics* , vol. 37, no. 16, pp.
2225–2230, 02 2021. [Online]. Available:
https://doi.org/10.1093/bioinformatics/btab102
- [21]
P. Chanda, E. Elhaik, and J. S. Bader, “Hapzipper: sharing hapmap populations
just got easier,” *Nucleic acids research* , vol. 40, no. 20, pp.
e159–e159, 2012.
- [22]
F. Mentzer, L. Van Gool, and M. Tschannen, “Learning better lossless
compression using lossy compression,” in *2020 IEEE/CVF Conference on
Computer Vision and Pattern Recognition (CVPR)* , June 2020, pp. 6637–6646.
- [23]
D. Taubman, A. Naman, R. Mathew, M. Smith, O. Watanabe, and P. Lemieux, “High
throughput jpeg 2000 (htj2k): Algorithm, performance and potential,”
*International Telecommunications Union (ITU)* , pp. 15 444–15, 2019.
- [24] A. Pranckevičius, “Lossless float image compression,” 07 2025. [Online]. Available: https://aras-p.info/blog/2025/07/08/Lossless-Float-Image-Compression/
- [25] A. Kapoulkine, “meshoptimizer,” https://github.com/zeux/meshoptimizer, 2017.
- [26] M. Hershcovitch, L. Choshen, A. Wood, I. Enmouri, P. Chin, S. Sundararaman, and D. Harnik, “Lossless and near-lossless compression for foundation models,” 2024. [Online]. Available: https://arxiv.org/abs/2404.15198
- [27]
M. Collins, “Computational graphs, and backpropagation,” *Lecture Notes,
Columbia University* , pp. 11–23, 2018.
- [28]
C. Shannon, “A mathematical theory of communication,” *The Bell System
Technical Journal* , vol. 27, pp. 379–423, 1948.
- [29]
D. A. Huffman, “A method for the construction of minimum-redundancy codes,”
*Proceedings of the IRE* , vol. 40, no. 9, pp. 1098–1101, 1952.
- [30]
I. H. Witten, R. M. Neal, and J. G. Cleary, “Arithmetic coding for data
compression,” *Commun. ACM* , vol. 30, no. 6, p. 520–540, Jun. 1987.
[Online]. Available: https://doi.org/10.1145/214762.214771
- [31] J. Duda, “Asymmetric numeral systems: entropy coding combining speed of huffman coding with compression rate of arithmetic coding,” 2014. [Online]. Available: https://arxiv.org/abs/1311.2540
- [32] Y. Collet, “Finite state entropy - a new breed of entropy coder,” https://fastcompression.blogspot.com/2013/12/finite-state-entropy-new-breed-of.html, 2013.
- [33] M. Burrows and D. J. Wheeler, “A block-sorting lossless data compression algorithm,” Digital Equipment Corporation, Systems Research Center, Palo Alto, CA, SRC Research Report 124, May 1994, sRC Research Report 124. [Online]. Available: https://www.cs.jhu.edu/˜langmea/resources/burrows_wheeler.pdf
- [34]
J. L. Bentley, D. D. Sleator, R. E. Tarjan, and V. K. Wei, “A locally adaptive
data compression scheme,” *Commun. ACM* , vol. 29, no. 4, p. 320–330,
Apr. 1986. [Online]. Available: https://doi.org/10.1145/5684.5688
- [35]
A. Robinson and C. Cherry, “Results of a prototype television bandwidth
compression scheme,” *Proceedings of the IEEE* , vol. 55, no. 3, pp.
356–364, 1967.
- [36]
J. Ziv and A. Lempel, “A universal algorithm for sequential data
compression,” *IEEE Transactions on Information Theory* , vol. 23,
no. 3, pp. 337–343, 1977.
- [37] Y. Collet, “LZ4 - Extremely fast compression,” https://github.com/lz4/lz4, self-published, Open source project, 2011.
- [38] S. H. Gunderson, “Snappy: A fast compressor/decompressor,” 2011, open source project. [Online]. Available: https://github.com/google/snappy
- [39] P. Deutsch, “DEFLATE compressed data format specification version 1.3,” Internet Requests for Comments, RFC Editor, RFC 1951, May 1996, rFC1951. [Online]. Available: https://www.rfc-editor.org/rfc/rfc1951.txt
- [40] L. P. Deutsch, “GZIP file format specification version 4.3,” RFC 1952, May 1996. [Online]. Available: https://datatracker.ietf.org/doc/html/rfc1952
- [41] J. Alakuijala and Z. Szabadka, “Brotli compressed data format,” Internet Requests for Comments, RFC Editor, RFC 7932, July 2016. [Online]. Available: https://www.rfc-editor.org/rfc/rfc7932.txt
- [42] Y. Collet and M. Kucherawy, “Zstandard compression and the application/zstd media type,” Internet Requests for Comments, RFC Editor, RFC 8878, October 2018, rFC8878. [Online]. Available: https://www.rfc-editor.org/rfc/rfc8878.txt
- [43] I. Pavlov, “LZMA algorithm description,” 2013, 7-zip documentation. [Online]. Available: https://www.7-zip.org/7z.html
- [44]
J. Cleary and I. Witten, “Data compression using adaptive coding and partial
string matching,” *IEEE Transactions on Communications* , vol. 32,
no. 4, pp. 396–402, 1984.
- [45]
G. V. Cormack and R. N. S. Horspool, “Data compression using dynamic markov
modelling,” *The Computer Journal* , vol. 30, no. 6, pp. 541–550, 12
1987. [Online]. Available: https://doi.org/10.1093/comjnl/30.6.541
- [46] M. Mahoney, “The PAQ data compression programs,” 2013, website documenting various PAQ iterations. [Online]. Available: http://mattmahoney.net/dc/paq.html
- [47] T. Boutell, “PNG (Portable Network Graphics) Specification Version 1.0,” RFC 2083, March 1997. [Online]. Available: https://www.rfc-editor.org/info/rfc2083
- [48] T. B. D. Team, “Blosc documentation,” 2010. [Online]. Available: https://www.blosc.org
- [49] A. S. Foundation, “parquet-format,” https://parquet.apache.org/docs/file-format/data-pages/compression/, Apache Software Foundation, Tech. Rep., 2024.
- [50] T. B. D. Team, “Btune: Making compression better,” https://blosc.org/pages/btune, IronArray SLU, Tech. Rep., 2023.
- [51] M. Mahoney, “The zpaq compression algorithm,” self-published, Technical Report ZPAQ-2015-12-29, Dec. 2015. [Online]. Available: https://mattmahoney.net/dc/zpaq_compression.pdf
- [52]
C. Dyer, Y. Goldberg, and G. Neubig, “Practical neural networks for NLP:
From theory to code,” in *Proceedings of the 2016 Conference on
Empirical Methods in Natural Language Processing: Tutorial Abstracts* ,
B. Yang and R. Hwa, Eds. Austin,
Texas: Association for Computational Linguistics, Nov. 2016. [Online].
Available: https://aclanthology.org/D16-2001/
- [53] S. Deorowicz, “Universal lossless data compression algorithms,” Ph.D. dissertation, Silesian University of Technology, 2003.
- [54] “Sao star catalog,” 2002. [Online]. Available: http://tdc-www.harvard.edu/software/catalogs/sao.html
- [55] J. Smit, “Binance full history,” https://www.kaggle.com/datasets/jorijnsmit/binance-full-history, 2025.
- [56] “TLC trip record data,” https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page, 2022.
- [57] H. Hersbach, B. Bell, P. Berrisford, G. Biavati, A. Horányi, J. Muñoz Sabater, J. Nicolas, C. Peubey, R. Radu, I. Rozum, D. Schepers, A. Simmons, C. Soci, D. Dee, and J.-N. Thépaut, “Era5 hourly data on single levels from 1940 to present,” 2023.
- [58] U. C. Bureau, “2020 census privacy-protected microdata file (ppmf) readme,” 2024. [Online]. Available: https://www2.census.gov/programs-surveys/decennial/2020/data/privacy-protected-microdata-file/2024-08-05-privacy-protected-microdata-file-README.pdf
- [59] A. C. S. Office, “5-year pums data (2023),” 2024. [Online]. Available: https://www2.census.gov/programs-surveys/acs/data/pums/2023/5-Year/
- [60]
K. Deb, A. Pratap, S. Agarwal, and T. Meyarivan, “A fast and elitist
multiobjective genetic algorithm: Nsga-ii,” *IEEE Transactions on
Evolutionary Computation* , vol. 6, no. 2, pp. 182–197, 2002.
- [61]
J. R. Koza, *Genetic Programming: On the Programming of Computers by Means
of Natural Selection* . Cambridge, MA,
USA: MIT Press, 1992.
- [62]
G. Jeong, B. Sharma, N. Terrell, A. Dhanotia, Z. Zhao, N. Agarwal,
A. Kejariwal, and T. Krishna, “Understanding data compression in
warehouse-scale datacenter services,” in *2022 IEEE International
Symposium on Performance Analysis of Systems and Software (ISPASS)* , 2022,
pp. 221–223.
- [63] M. Slee, A. Agarwal, and M. Kwiatkowski, “Thrift: Scalable cross-language services implementation,” Facebook, Technical Report, 2007. [Online]. Available: https://thrift.apache.org/static/files/thrift-20070401.pdf
- [64] (2024) nimble. https://github.com/facebookincubator/nimble. Facebook Incubator.
- [65] M. Karpathiotakis, D. Wernli, and M. Stojanovic. (2019) Scribe: Transporting petabytes per hour via a distributed, buffered queueing system. https://engineering.fb.com/2019/10/07/core-infra/scribe/.
- [66] W. F. Handte, Y. Collet, and N. Terrell, “5 ways Facebook improved compression at scale with Zstandard,” 2018. [Online]. Available: https://engineering.fb.com/2018/12/19/core-infra/zstandard/
