---
id: collect-261121-veille-ia/langages-dev/max-nightly-26-7-0-dev2026102105-mojo-1-2-0-dev2026102105-released
title: "MAX Nightly 26.7.0.dev2026102105 (Mojo 1.2.0.dev2026102105) Released"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["tile", "mojo", "dev2026102105", "each", "mode", "nightly", "released", "added"]
source: https://forum.modular.com/t/max-nightly-26-7-0-dev2026102105-mojo-1-2-0-dev2026102105-released/3574
source_anchor: ""
source_lines: [1, 85]
section: "Langages_Dev"
---
A new nightly version has been released! 

See the quickstart guide for installation instructions: Quickstart | MAX

MAX changelog updates:

## Models

## Inference server

### Server metrics

- Added `maxserve_request_client_disconnect_cancellations_total` , which
 counts streamed responses cut short by a client disconnect and completion
 requests abandoned while waiting for a result, split by`phase` . A
 mid-stream disconnect was previously invisible:
 `maxserve_request_count_total` records it under the status code already
sent.

## `max` CLI

## Python API

- 
Added `max.driver.Buffer.memset(value)` to fill a contiguous buffer or
slice in place with a byte value, without allocating a source buffer.
- 
`generate()` ,`GenerateIterator` ,`AutoModel.generate()` and
 `AutoModel.generate_iter()` take`messages` and`images` , so they run chat
 and image prompts on models such as Gemma 3 and InternVL.
 `GenerateResult.prompt_lengths` gives each prompt’s length.`AutoModel`
turns the vision encoder cache off by default.
- 
Added `AutoProcessor` to`max.pipelines` , which turns chat conversations
 with inline images into the`messages` and`images` that
 `AutoModel.generate()` takes.
- 
`AutoPipeline` now takes a chat conversation, or a list of them, with
images inline, and returns only the new text for each one.

## C API

## Kernels and GPU programming

- 
The deprecated `LayoutTensor` and`LayoutTensorIter` types have been removed
 from the`layout` package, along with the`to_layout_tensor()` and
 `lt_to_tt()` conversions. Use`TileTensor` and`TileTensorIter` instead.
 `ThreadScope` now lives in`layout.tile_io` .
- 
`TileTensor.tile()` on a nested layout now indexes each mode first leaf
 first, as`crd2idx` and the CuTe`local_tile` operation do: tile`c` of size
 `t` starts at element`c * t` of the mode, and the tile keeps the leading
 leaves it spans, with their strides. Before, it treated the first leaf of
 each mode as the tile grid and the second as the fragment. To extract
 fragments, put the fragment first in each mode, as`blocked_product()` does:
 `((3, 2), (4, 2))` rather than`((2, 3), (2, 4))` . A tile size that doesn’t
split a nested mode on leaf boundaries is now a compile-time error.
- 
Removed the `max` ,`min` ,`sum` ,`product` and`mean` overloads in
 `max.algorithm` that take an input shape, an`input_fn` , an`output_fn` and
 a`reduce_dim` . To reduce along an axis, use the graph ops (`ops.sum` ,
 `ops.max` ,`ops.min` ,`ops.mean` ,`ops.prod` ), or call`map_reduce` once
 per row. The`Span` overloads and the 1-D`sum` and`mean` overloads that
 take a length and an`input_fn` are unchanged.

## Tools

- Added the ability to install `max-serve` and`max-benchmark` as Conda extras,
 using`max[extras=[serve]]` and`max[extras=[benchmark]]` respectively.
 `max[extras=[all]]` installs both plus`mojo` .

Mojo changelog updates:

- [Driver][Kernels] Add Apple M6 Support

Raw MAX diff: https://github.com/modular/modular/compare/5c715c9bd628d626d98c6af4da1ff94eb54cd41e...a1ccd09545970f73bd33527ef027d445b6ad0980

Current Mojo changelog: https://github.com/modular/modular/blob/main/Mojo/docs/site/nightly-changelog.md

             
              
                  
                  
                1 Like
