---
id: collect-261121-veille-ia/modeles-open/reflection-beam-a-501b-open-weight-model-under-apache-2-0
title: "Reflection Beam: A 501B Open-Weight Model Under Apache 2.0"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "reflection", "beam", "open", "weights", "under", "apache", "build"]
source: https://www.digitalapplied.com/blog/reflection-beam-open-model-apache-2-licence-hardware
source_anchor: ""
source_lines: [1, 143]
section: "Modeles_Open"
---
Reflection released Beam's weights on October 16, 2026, eleven days after introducing the model. Its launch post calls it "our first open-weight model", built for coding, reasoning and long-horizon agent work. The weights are a 501-billion-parameter mixture-of-experts transformer with about 23 billion parameters active per token, a one-million-token context, and text in with text out. They ship under Apache 2.0, the permissive open-source licence, which means no revenue threshold, no attribution line in your product and no separate agreement for anyone.

The timing makes the licence the story. Meta released Muse Spark Open the day before, a larger model that takes images, video and audio, under a custom licence with an attribution rule and a revenue clause for model-as-a-service providers. We covered it in our Muse Spark Open release post. Two US labs shipped frontier-scale open weights in two days, and they made opposite licensing choices. The side-by-side comparison is in Reflection Beam vs Muse Spark Open; this post is about Beam on its own terms.

Every source below was read on October 19. Reflection's figures are Reflection's: the benchmarks were run by Reflection, at its chosen settings, and nobody outside the company has reproduced them. Artificial Analysis published its own independent index score for Beam on October 16; the companion comparison sets it beside Muse Spark Open's.

1. 01Apache 2.0, with no strings attached.No revenue threshold, no attribution rule, no separate agreement. Reflection's model card also says it does not prohibit distillation, training other models on Beam's outputs, fine-tuning, quantisation or merging.
2. 02501B on disk, 23B per token.Only 8 of 256 experts run for each token, so Beam computes like a much smaller model. All 256 stay in memory, so you still need the memory of a 501B model.
3. 03Four B300s for the smallest official build.The NVFP4 checkpoint is 273 GiB and Reflection's serving guides assume four NVIDIA B300 GPUs. On Hopper hardware, Hugging Face's guide uses the FP8 build on eight H200s.
4. 04The 4-bit build keeps most of the quality.On Reflection's own runs, NVFP4 lands within about two and a half points of BF16 on most rows, and above it on a few. That is the build most teams will actually serve.
5. 05Set the reasoning effort yourself.Hugging Face's write-up gives medium as Beam's default; OpenRouter lists max as the route default. The same prompt can cost very different amounts depending on which door you use.

## 01 — ReleaseWhat shipped on *October 16*.

Reflection shipped four things together. The weights, on Hugging Face, in three numeric formats: BF16, the full-precision release; FP8, an 8-bit build; and NVFP4, NVIDIA's 4-bit format for its Blackwell GPUs. A small draft model called DFlash2 for speculative decoding, a technique where a cheap model guesses the next several tokens and the large model checks them in one pass. A model card with Reflection's final benchmarks. And a safety technical report setting out how the model was tested before release.

Around the weights sits what the launch post calls "an open operating system for agents": more than 40 partners and integrations, reference agent integrations, deployment containers and customisation pipelines. Reflection's own line is that "the model is the kernel". Day-one support covers Hugging Face Transformers, vLLM and SGLang, and the model ships with Mirror CLI, Reflection's open-source coding-agent harness, which pairs a Python front end with a Rust agent core.

Why the bundle matters: open weights alone leave the hard parts to the buyer. A team that downloads 273 GiB still has to pick an inference engine, tune it for the hardware and wire the model into a coding agent. Shipping containers, a drafter and a harness on day one removes weeks from that path. The failure case is assuming the bundle is finished. It is a first release from a new lab; treat the harness and partner integrations as starting points to test, not as production-hardened infrastructure.

It is for teams that want a frontier-scale model on their own hardware, infrastructure, or cloud, and want to customize it to their own use cases.Ben Burtenshaw, Merve Noyan and Pedro Cuenca, Hugging Face blog, October 16, 2026

## 02 — ArchitectureWhat 501B and 23B *mean*.

A mixture-of-experts model splits most of its layers into many smaller networks, called experts, and a router picks a few of them for each token. Beam has 52 layers, 49 of them with 256 experts each, and the router selects 8 per token. Reflection's model card puts the totals at 501.3 billion parameters, 23.4 billion of them active. The card also lists a vocabulary of about 152,000 tokens, a modified Harmony chat format, and a knowledge cutoff of June 30, 2026.

The two numbers answer two different questions. Active parameters set the compute per token, and so the speed and the serving cost per request. Total parameters set the memory, because every expert has to be loaded even though only a few run at once. Hugging Face's write-up puts it plainly: "deployment needs the memory of a 501B model but runs at the speed of a 23B one."

The long context has its own memory trick. Most of Beam's layers use sliding-window attention, which only looks back over the previous 512 tokens; one layer in six, plus the last, attends to the whole sequence. Hugging Face calculates that this keeps the attention cache at about 4.6 GiB for a 128,000-token conversation, against about 26 GiB if every layer kept every token. The practical effect is that long agent sessions and concurrent users fit on the same GPUs that hold the weights.

The failure case is reading 23B as a laptop model. It is not. The active count tells you how fast a token comes out once the weights are loaded; it says nothing about whether they fit. Budget memory against 501B, and speed against 23B.

## 03 — LicenceWhat Apache 2.0 *allows*.

Apache 2.0 is a standard open-source licence that permits commercial use, modification and redistribution, with a notice file and a patent grant. It has no clause that changes at a revenue level, no rule that your product has to name the model, and no restriction on who may offer the model as a hosted service. Muse Spark Open, Kimi K3 and GLM 5.3 all carry custom terms instead; compare them in our audit of 2026 open-weight licences.

Reflection's model card goes further than the licence text needs to. It says Reflection does not prohibit distillation, using the model's outputs to train other models, fine-tuning, parameter-efficient adaptation, quantisation, merging, defensive security research on systems you are authorised to test, or good-faith adversarial evaluation. The card adds that it is informational and does not modify the licence, and points to a usage policy worth reading before a launch.

##### Build on it, *no notice*

An internal coding agent, a support assistant or a document pipeline built on Beam owes Reflection nothing beyond keeping the licence notice with any copy of the weights you redistribute.

##### Sell access to it

A platform that resells Beam as an API has no revenue threshold to watch. Under Muse Spark Open's licence, providers above $20M in revenue need a separate agreement with Meta.

##### Distil or fine-tune it

Reflection says it does not prohibit distillation or training other models on Beam's outputs. Teams building a smaller in-house model can use Beam as the teacher without a terms question.

The failure case is assuming permissive means unreviewed. The weights are yours to use, but the data you run through them, the outputs your product shows and the sector rules you work under are still yours to handle. Apache 2.0 removes the vendor-terms question, not the compliance one.

## 04 — HardwareWhat it takes to *run*.

Reflection's serving documentation lists the download sizes exactly: 933.81 GiB for BF16, 468.72 GiB for FP8 and 273 GiB for NVFP4. Its serving guides for SGLang, vLLM and TokenSpeed use the NVFP4 build and assume four NVIDIA B300 SXM GPUs linked by NVLink. The B300 is NVIDIA's Blackwell Ultra part, so this is current-generation data-centre hardware, rented or owned.

Hugging Face's guide fills in the alternatives. NVFP4 fits on four B300s. Two B200s could technically hold the weights, but with little room left for the cache, so the guide recommends four. On the older Hopper generation it serves the FP8 build on eight H200s. The BF16 reference runs across sixteen H200s, two nodes of eight.

| Sources: Reflection developer documentation, last modified October 16, 2026; Hugging Face technical write-up, October 16, 2026. Read October 19. |  |  |  | 
|---|---|---|---|
| Build | Download | Runs on | Stated by | 
|---|---|---|---|
| NVFP4 | 273 GiB | 4 × B300 (or 4 × B200) | Reflection docs, HF blog | 
| FP8 | 468.72 GiB | 8 × H200 on Hopper | Reflection docs, HF blog | 
| BF16 | 933.81 GiB | 16 × H200, two nodes | Reflection docs, HF blog | 
| LoRA fine-tuning | BF16 weights | 1 node of 8 × B300, ~19,000 tokens/s | HF blog | 

The example that makes this concrete: an agency that already rents a four-GPU Blackwell node for image or video work can load Beam on it in NVFP4 with room for long sessions. A team on Hopper needs twice the GPUs for the 8-bit build. The failure case is the one our open-weight self-hosting guide keeps returning to: buying or renting for the weights and forgetting the cache and the headroom for more than one user at a time.

## 05 — QuantisationWhat the 4-bit build *gives up*.

Quantisation stores weights in fewer bits to cut memory, and it usually costs some accuracy. Reflection published every benchmark in all three formats, which is the useful part of its card for anyone self-hosting: you can see what moving from BF16 to the 4-bit build costs on Reflection's own runs before you commit to hardware. All runs used temperature 0.7 and top-p 0.99.

| Source: Reflection's Beam model card, benchmark tables for BF16 and NVFP4, read October 19, 2026. Reflection-run; temperature 0.7, top-p 0.99. Not independently reproduced. |  |  |  | 
|---|---|---|---|
| Benchmark | BF16 | NVFP4 | What it tests | 
|---|---|---|---|
| SWE-bench Verified | 81.6% | 80.7% | Fixing real GitHub issues | 
| Terminal-Bench 2.1 | 80.1% | 78.4% | Tasks in a command-line environment | 
| SWE-Bench Pro v2-Hard | 76.5% | 78.6% | Harder software-engineering tasks | 
| MCP Atlas | 80.8% | 80.5% | Tool use through MCP servers | 
| GPQA Diamond | 90.6% | 89.3% | Graduate-level science questions | 
| SciCode | 49.2% | 46.9% | Scientific programming | 

On these rows the 4-bit build stays within about two and a half points of BF16, and comes out ahead on SWE-Bench Pro v2-Hard, which is noise around the same level rather than a gain. That matters because NVFP4 is the build that fits on four GPUs, so it is the one most teams will run. The comparison of Beam against other models on the same benchmark names, including where it trails, is in the companion comparison.

The failure case is treating these as your numbers. They are Reflection's runs on public suites, and a suite's score says little about your repository. Run your own tasks at the build and effort level you plan to serve, and keep the result next to the configuration that produced it.

## 06 — SettingsPin the *effort* level.

Beam is a reasoning model: it writes hidden working before its answer, and you choose how much. Hugging Face's write-up lists five values, low, medium, high, xhigh and max, with medium as the default. A `preserve_thinking` option keeps that reasoning in context across turns, and tool calls use a dedicated grammar so the model's calls parse reliably. The tokenizer carries a response template that splits reasoning, answer and tool calls into separate fields.

The catch is that defaults differ by door. OpenRouter's listing for `reflection/beam-1.0-501b-a23b`, captured October 19, marks reasoning as mandatory with a default effort of max, as does Reflection's own API reference. A team that tries the model through OpenRouter at its default and then self-hosts at medium is comparing two different configurations, and paying for two very different numbers of reasoning tokens.

The example: an agent that classifies support tickets needs a short answer thousands of times a day, and low effort is the right setting for it. A refactoring agent on a large codebase earns high or above. The failure case is leaving max on for the first and paying for reasoning nobody reads. Set effort in the request, record it beside every evaluation, and do not rely on any default.

## 07 — AccessWhere to call it *today*.

Most teams will try Beam without downloading anything. Reflection runs a first-party API in public beta, free to start, and has not published a price for it. Hosted inference partners serve it too. The one price list we can cite is OpenRouter's, where four providers listed the model on October 19 at prices that differ by a factor of five on input.

| Sources: Reflection launch post, October 16, 2026; OpenRouter endpoint listing for reflection/beam-1.0-501b-a23b, captured October 19, 2026 (CoreWeave re-checked October 20), USD per million tokens. Route prices change without notice. |  |  |  | 
|---|---|---|---|
| Door | Format | Input / output | Note | 
|---|---|---|---|
| Reflection API | not stated | free beta | No published price yet | 
| Nebius via OpenRouter | FP8 | $0.25 / $1.00 | 131,072 max output | 
| Together via OpenRouter | not stated | $0.40 / $1.20 | Cache reads $0.04 | 
| CoreWeave via OpenRouter | FP4 | $0.30 / $1.00 | Headline route; $1.00 / $3.00 on October 19 | 
| Baseten via OpenRouter | FP4 | $1.30 / $4.30 | Cache reads $0.13 | 

A fivefold spread for the same weights reflects providers picking different quantisations, hardware and margins. It is also a trap: a price quoted as "Beam costs $X" without a provider and a format means nothing. The failure case is benchmarking on one provider and deploying on another that serves a different quantisation. Pin the provider in your router settings once you have chosen.

## 08 — FitWho it *suits*, from the pages read.

Beam is text-only, predominantly English, tuned for coding and agent work, and licensed with no strings. That points at a clear set of buyers and leaves out others. The routing below is ours, built from Reflection's documentation and the licence, not from a trial of our own.

##### Platforms reselling a model

Anyone offering a hosted model to customers avoids the revenue clause and attribution rule that come with Muse Spark Open. Apache 2.0 carries neither.

##### Self-hosted coding agents

Teams that must keep code on their own hardware get a frontier-scale coding model that fits on one four-GPU Blackwell node in NVFP4, with a drafter for speed.

##### Distillation and fine-tuning

Teams training a smaller in-house model can use Beam as the teacher without a terms question, and LoRA fine-tuning fits on one eight-GPU node.

##### Images, audio or non-English work

Beam takes text only and was developed predominantly for English. Screenshot-driven agents, document images and multilingual products need a different model.

If you are choosing a model for an agent rather than a licence, our October release tracker lists the month's other major releases, including Meta's larger multimodal release. Help testing a self-hosted model against your own workload is part of our AI transformation work.

## 09 — ConclusionThe licence is the *headline*.

### Start on a hosted route at a pinned effort level, measure reasoning tokens per finished task, and only then price a four-GPU node.

Beam is a 501B mixture-of-experts with about 23B active parameters, a one-million-token context and text-only input, released by Reflection on October 16 under Apache 2.0. The licence is the part that stands out: no revenue trigger, no attribution rule, and a model card that explicitly does not prohibit distillation or training on its outputs.

Running it yourself means 273 GiB for the 4-bit build on four Blackwell-generation GPUs, or the 8-bit build on eight H200s. On Reflection's own runs, the 4-bit build stays within a couple of points of full precision on most rows. Every benchmark is Reflection's, and the three-times token-efficiency claim is one to measure, not to assume.

The cheapest way to find out whether it fits is a hosted trial at a fixed effort level on your own tasks, with the provider and format written down. If the results and the token counts hold up, the licence makes the self-hosting decision simpler than the custom terms on Muse Spark Open, Kimi K3 or GLM 5.3.
