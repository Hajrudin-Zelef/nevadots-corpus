---
id: collect-261121-veille-ia/langages-dev/max-nightly-26-7-0-dev2026102205-mojo-1-2-0-dev2026102205-released
title: "MAX Nightly 26.7.0.dev2026102205 (Mojo 1.2.0.dev2026102205) Released"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["dtype", "mojo", "stdlib", "kernel", "dev2026102205", "request", "name", "logits"]
source: https://forum.modular.com/t/max-nightly-26-7-0-dev2026102205-mojo-1-2-0-dev2026102205-released/3577
source_anchor: ""
source_lines: [1, 60]
section: "Langages_Dev"
---
A new nightly version has been released! 

See the quickstart guide for installation instructions: Quickstart | MAX

MAX changelog updates:

- 
InternVL and Gemma 3 multimodal models now return log probabilities, so
 `logprobs` in`max serve` and`output_logprobs` in`generate()` work for
them.
- 
`/v1/completions` now returns a 400 error for`echo` with`logprobs` on a
 server started without`--enable-echo` , when the model computes log
 probabilities. The request used to crash the model worker. Other models
 still return no log probabilities, and`echo` without`logprobs` has no
effect.
- 
MAX Serve’s span attributes now sit under the entity they describe:
 `max.request_id` is now`max.request.id` ,`max.batch_id` is`max.batch.id` ,
 and the other request and batch attributes move under`max.request.*` and
 `max.batch.*` .`kernel_agg:<name>` spans carry the kernel name in
 `max.kernel.op` instead of`max.kernel.name` ; per-kernel spans keep
 `max.kernel.name` . Update trace queries that filter on the old names.
- 
`max.nn.Transformer` ,`DistributedTransformer` ,`logits_postprocess()` and
 `distributed_logits_postprocess()` return a
 `max.nn.graph_groups.DecoderOutputs` of named groups instead of a tuple.
 Read the logits as
 `outputs.logits["logits"]` rather than`outputs[1]` , and pass the outputs to
 `Graph.output(outputs)` rather than unpacking them. The graph’s outputs keep
 their order, and`Graph.output_tree` returns them by name.
- 
`ops.matmul()` takes an`out_dtype` keyword argument that sets the result
 dtype. The inputs keep their own dtypes and are no longer cast to a common
 one, so`bfloat16` inputs with`out_dtype=DType.float32` keep the float32
 accumulator. Without`out_dtype` ,`ops.matmul()` , the`@` operator and
 `max.experimental.functional` ’s`matmul()` require both inputs to share a
 dtype, which the result keeps, so`max.nn.Linear` raises when its input and
 weight dtypes differ.
 `max.experimental.nn.Linear` casts its input to the weight’s dtype, so a
 float32 input to a bfloat16`Linear` now gives bfloat16 instead of float32.
- 
Removed the `blocking_sync` ,`disable_timing` and`interprocess` parameters
 from the Mojo`DeviceContext.create_event()` , along with`EventFlags` . Events
 never record timing; use`DeviceContext.execution_time()` to time kernels.

Mojo changelog updates:

- [stdlib] Move AMD-only intrinsics to `max.gpu.intrinsics`
- [mojo-compiler][stdlib] Deprecate `@explicit_destroy` on structs
- [mojo-lang][stdlib] Remove the legacy kernel capture ABI (#105477)
- [stdlib] Add an owning FileDescriptor type
- [mojo-lang][stdlib] Remove legacy closures (#105361)
- [mojo-lang] Fix import leaks from precompiled packages
- [stdlib] stabilize some more list methods.
- [stdlib] Fix the `SIMD.slice` fast-path offset check

Raw MAX diff: https://github.com/modular/modular/compare/a1ccd09545970f73bd33527ef027d445b6ad0980...c783ebd98c26aa012cee07ecee96de212f19f22e

Current Mojo changelog: https://github.com/modular/modular/blob/main/Mojo/docs/site/nightly-changelog.md
