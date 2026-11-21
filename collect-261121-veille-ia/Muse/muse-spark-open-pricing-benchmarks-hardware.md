---
id: collect-261121-veille-ia/muse/muse-spark-open-pricing-benchmarks-hardware
title: "Muse Spark Open: Pricing, Benchmarks, Hardware"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "spark", "open", "tokens", "model", "output", "index", "task"]
source: https://capitalandcompute.net/blog/muse-spark-open-pricing-benchmarks/
source_anchor: ""
source_lines: [1, 153]
section: "Muse"
---
Muse Spark Open costs $1.25 per million input tokens and $4.25 per million output tokens on every host that serves it, and it scores 39 on the independent Artificial Analysis index, the highest score of any US open-weight model. A task costs $0.84. DeepSeek V4.1 Flash scores the same 39 for $0.27, so the case for Meta’s model is the weights and the licence, not the price.

Meta released Muse Spark Open on October 15, 2026, calling it “the most capable US open-weight model to date”. It is a sparse mixture of experts with 816 billion parameters, 42 billion of them active per token, and the first Muse Spark whose weights anyone can download. Meta’s closed models, Muse Wonder and Muse Spark 1.3, stay on its own API.

|  | Muse Spark Open | 
|---|---|
| Input, per Mtok | $1.25 (every host on OpenRouter) | 
| Output, per Mtok | $4.25 | 
| Cached input, per Mtok | $0.15 (four of the five hosts) | 
| Meta Model API | Not offered | 
| Hosts at launch | Baseten, Fireworks, Groq (preview), Modal, Nebius, Together | 
| Weights | Hugging Face, SageMaker, Google Cloud Model Garden, Foundry | 
| Licence | Muse Model License (custom, not Apache 2.0) | 
| Size | 816B total, 42B active | 
| Context window | 1,048,576 tokens | 
| AA Intelligence Index | 39 (xhigh effort) | 
| AA cost per index task | $0.84 | 
| Smallest Q4 file (Unsloth GGUF) | 505.5 GB | 

## How much does Muse Spark Open cost?

$1.25 per million input tokens, $0.15 per million cached input tokens and $4.25 per million output tokens. Meta does not sell it on the Meta Model API, so the price is whatever hosts charge, and at launch they all charge the same. OpenRouter’s Muse Spark Open listing shows Modal, Together, Nebius, Fireworks and Baseten at identical rates on October 16, with Nebius the only one listing no cached-input price. Groq is serving it as a limited preview, according to Meta’s developer post.

That is exactly the rate card of the closed Muse Spark 1.3 on Meta’s own API, which the Muse Spark 1.3 pricing breakdown covers. Meta’s contributor tier for 1.3, $0.10 input and $0.20 output in exchange for Meta training on your traffic, has no counterpart here: the hosts set the price, and none offers a data-for-discount deal.

The hosted rate is a starting point that will move. Open weights let any provider compete on price, and five hosts at one identical number on day one usually means a price anchored to Meta’s own, not a floor.

## What does a task cost on Muse Spark Open?

$0.84 per task on the Artificial Analysis Intelligence Index, at xhigh effort, against the $1.25 and $4.25 host rate. Artificial Analysis measured 110 million output tokens to run the index and 102 output tokens per second.

That makes it cheaper per task than every open-weight model above it except one, and more expensive than the one that matches it:

| Model (open weights) | AA index | Cost per index task | Output tokens | Price per Mtok (in / out) | 
|---|---|---|---|---|
| GLM-5.3 (max) | 45 | $2.01 | 210M | $1.40 / $4.40 | 
| Kimi K3 (max) | 44 | $2.00 | 160M | $3.00 / $15.00 | 
| Qwen3.8 2.4T A95B | 40 | $2.16 | 170M | $2.00 / $6.00 | 
| Muse Spark Open (xhigh) | 39 | $0.84 | 110M | $1.25 / $4.25 | 
| DeepSeek V4.1 Flash (max) | 39 | $0.27 | 250M | $0.30 / $1.20 | 

Read off each model’s Artificial Analysis page on October 16, 2026: GLM-5.3, Kimi K3, Qwen3.8 2.4T A95B and DeepSeek V4.1 Flash.

Muse Spark Open is the most economical with tokens on that list, which is why it lands at $0.84 despite a rate card above DeepSeek’s. DeepSeek V4.1 Flash spends more than twice the output tokens and still costs less than a third as much per task, because its rates are a quarter of Meta’s. If the index is your measure and provenance does not matter, DeepSeek is the cheaper route to 39.

**Update, October 24, 2026:** the DeepSeek gap widened. DeepSeek replaced the build behind `deepseek-flash` with DeepSeek-V4.1-Flash-1017 on October 17 at unchanged rates, and Artificial Analysis now scores it 45 (max) for $0.26 a task, six points above Muse Spark Open. The October 16 reading above is left as published.

Against Meta’s own closed model the trade runs the other way. Muse Spark 1.3 at max effort scores 48 for $1.60 a task on its Artificial Analysis page: nine more points for 76 cents more, at the same price per token. If you are calling an API anyway and do not need the weights, 1.3 is the stronger model for the money.

## Can you run Muse Spark Open locally?

Yes, but not on one machine at the 4-bit quality most people run local models at. The smallest 4-bit file is Unsloth’s UD-Q4_K_XL GGUF at 505.5 GB, measured from the Unsloth Muse Spark Open GGUF repository on October 16. A 512 GB Mac Studio cannot hold that plus the operating system and a working context. The 2-bit UD-Q2_K_XL file, at 311.2 GB, fits one.

| Muse Spark Open file size by quantization, against machine memory |  | 
|---|---|
| Item | Size | 
|---|---|
| UD-IQ1_S (1-bit) | 175.6 GB | 
| UD-IQ2_XXS (2-bit) | 225.6 GB | 
| UD-Q2_K_XL (2-bit) | 311.2 GB | 
| UD-Q3_K_XL (3-bit) | 401.5 GB | 
| UD-Q4_K_XL (4-bit) | 505.5 GB | 
| NVFP4 (Meta) | 530 GB | 
| BF16 (Meta) | 1,600 GB | 
| Memory line: 256 GB Mac Studio | 256 GB | 
| Memory line: 512 GB Mac Studio | 512 GB | 
| Memory line: 2 Mac Studios, 1 TB | 1,024 GB | 

Meta’s own answer is two machines. Its developer post runs the NVFP4 checkpoint, about 530 GB with only the experts quantized, across two M5 Ultra Mac Studios with 512 GB each, or four with 256 GB, joined over Thunderbolt 5 with MLX tensor parallelism. On one older 512 GB M3 Ultra, the Hugging Face launch post measured 18 tokens per second with a Q2_K build in llama.cpp and about 21 tokens per second generating with a 3-bit MLX build.

Context is the hidden cost. Hugging Face puts the key-value cache at about 250 KB per token in BF16, roughly 266 GB for the full 1M-token window, on top of the weights. A local build at 2 or 3 bits will run, but at a fraction of the context Meta advertises.

On data-centre hardware Meta lists a floor of four Blackwell Ultra GPUs or eight H200s for NVFP4; Hugging Face’s recipe for the full 1M context uses eight H200s. Whether owning that beats $0.84 a task is the break-even question in the self-hosted LLM cost per token analysis, and the local LLM hardware checker now includes Muse Spark Open.

## What does the Muse Model License allow?

Free use for most developers, with two strings. The Muse Model License on the model card requires any product, service or documentation built on the model to display “Built with Muse Spark Open”. And a company that sells API access to AI models, with more than $20 million in aggregate gross revenue in any 12 months, must notify Meta and sign a separate agreement before offering this one.

That revenue test is the clause to read. It targets model-as-a-service businesses, the hosts that compete with Meta’s own API, and exempts internal use, research, and products that use the model as a component. There is no monthly-active-user cap of the kind the old Llama licences carried. Meta claims no ownership of outputs, and the acceptable-use policy and an age floor of 18 still apply.

It is not open source in the Apache 2.0 sense. Thinking Machines’ Inkling ships under Apache 2.0 and DeepSeek V4.1 Flash under MIT, with no attribution or revenue clause. If your legal team requires a standard open-source licence, Muse Spark Open does not qualify; if it requires a US-built model you can run inside your own walls, nothing open scores higher today.

## Is Muse Spark Open better than Kimi K3 and GLM-5.3?

Not on the independent index: GLM-5.3 scores 45 and Kimi K3 44 to Muse Spark Open’s 39. On Meta’s own model card it leads both on tool use, no-tools knowledge and multimodal tests, and trails both on coding. Every score below is vendor-reported by Meta and not independently reproduced.

| Benchmark (Meta-reported) | Muse Spark Open (xhigh) | Kimi K3 (max) | GLM-5.3 (max) | Qwen3.8 2.4T (xhigh) | 
|---|---|---|---|---|
| MCP-Atlas | 89.2 | 82.3 | 84.2 | 84.5 | 
| JobBench | 61.8 | 54.3 | 61.4 | 52.7 | 
| HLE, no tools | 26.1 | 22.2 | 16.4 | 17.3 | 
| HLE, with tools | 61.0 | 56.0 | 62.5 | 56.2 | 
| OSWorld Verified | 80.4 | 84.8 | Not reported | Not reported | 
| Terminal-Bench 2.1 (Terminus 2) | 76.4 | 85.0 | 83.9 | 82.0 | 
| DeepSWE 1.1 | 60 | 68 | 67 | 57 | 
| GPQA Diamond | 89.0 | 93.5 | 91.7 | 93.5 | 
| GDPVal-AA v2 | 1401 | 1537 | 1653 | 1613 | 
| ZeroBench, with tools | 50.0 | 41.0 | Not reported | Not reported | 

The pattern is consistent: Muse Spark Open is an agent and vision model first and a coder second. Its Terminal-Bench 2.1 score is 8.6 points below Kimi K3’s, and its DeepSWE score 8 below. One row stands apart. On the StrongREJECT jailbreak test, where lower is safer, Meta reports 0.5 for Muse Spark Open against 62.9 to 66.6 for the three Chinese models, which matters if you deploy where outputs are not reviewed.

## How does Muse Spark Open compare with Mistral Large 4?

It scores one point higher, 39 to 38, and you can download it today. Mistral Large 4 is the bigger model, 1.05 trillion parameters with 52 billion active, and Mistral has promised its weights by the end of October without naming a licence. Mistral’s API sells Large 4 at a launch-sale $0.68 and $2.09 per million tokens, half its standard $1.36 and $4.18, and Artificial Analysis costs an index task at $1.13 on the standard rate.

So for the “Western open-weight” buyer the field on October 16 is Muse Spark Open with weights and a custom licence, Large 4 with an API and weights pending, and Reflection AI’s Beam, still waitlist-only with no independent score. The wider ranking is in the best open-weight AI models of 2026.

## Should you use Muse Spark Open?

It depends on why you want an open model. Four cases:

- **You need weights you can run inside your own network, from a US lab.** Use it. Nothing open from a US or European lab scores higher on the independent index, and the licence allows internal and product use without a revenue cap.
- **You want the cheapest route to this capability through an API.** Use DeepSeek V4.1 Flash, which reaches the same 39 for $0.27 a task, if your procurement rules allow a Chinese model.
- **You want the strongest Meta model and do not need weights.** Use Muse Spark 1.3 on Meta’s API: 48 for $1.60 a task at the same per-token rate.
- **You want the strongest open coder.** Kimi K3 and GLM-5.3 lead it on every coding row Meta itself published.

To price your own task shape, run it through the AI model comparison tool; every current Meta model is on the AI model tracker, and the rest of the month’s launches are in new AI models released in October 2026.

## How to get started with Muse Spark Open

1. **Try it hosted first.** Call`meta/muse-spark-open` through OpenRouter, or the same model on Fireworks, Together, Modal, Nebius or Baseten, at $1.25 and $4.25 per million tokens. This tells you whether the model fits your task before you buy any hardware.
2. **Check the licence against your business.** If you sell model API access and clear $20 million a year, talk to Meta first. Otherwise, add the “Built with Muse Spark Open” credit.
3. **Pick a build for your memory.** One 512 GB Mac Studio runs the 2-bit or 3-bit GGUF; the 4-bit and NVFP4 builds need two machines or a four-GPU Blackwell node.
4. **Budget context separately.** Plan for about 250 KB of key-value cache per token in BF16, and cap the context length on smaller machines.

## Frequently asked questions

- How much does Muse Spark Open cost?
- Hosted, $1.25 per million input tokens, $0.15 per million cached input tokens and $4.25 per million output tokens, the same at every host OpenRouter listed on October 16, 2026. Meta does not offer it on the Meta Model API. The weights are free to download under the Muse Model License.
- Is Muse Spark Open the best US open-weight model?
- On the Artificial Analysis Intelligence Index, yes. It scores 39 at xhigh effort, above Thinking Machines Inkling at 25 and Nvidia Nemotron 3 Ultra at 23. Every open-weight model that scores higher comes from a Chinese lab, led by GLM-5.3 at 45 and Kimi K3 at 44, and DeepSeek V4.1 Flash ties it at 39.
- Can Muse Spark Open run on a Mac Studio?
- On one 512 GB Mac Studio, only at 2 or 3 bits: the 2-bit UD-Q2_K_XL GGUF is 311.2 GB and the 3-bit UD-Q3_K_XL 401.5 GB. The 4-bit UD-Q4_K_XL file is 505.5 GB and does not leave room for the system and context. Meta runs the NVFP4 build across two 512 GB M5 Ultra Mac Studios.
- Is Muse Spark Open open source?
- It is open weights under a custom licence, not an open-source licence like Apache 2.0. The Muse Model License requires a visible "Built with Muse Spark Open" credit and a separate agreement with Meta for companies selling model API access with more than $20 million in gross revenue in any 12 months.
- Is Muse Spark Open the same model as Muse Spark 1.3?
- No. Muse Spark 1.3 is a closed model on the Meta Model API and scores 48 on the Artificial Analysis index at max effort. Muse Spark Open is a separate open-weight release that scores 39 at xhigh. Both are priced at $1.25 and $4.25 per million tokens.

## Sources

Meta (2026). *Muse Spark Open*. Meta research blog, October 15, 2026: 816B total and 42B active parameters, 1M-token context, host and download platforms, two-Mac-Studio deployment. https://research.meta.ai/blog/muse-spark-open Verified 2026-10-16.

Meta (2026). *Build with Muse Spark Open*. Meta developer blog: host list, quantized checkpoints (BF16 about 1.6 TB, NVFP4 about 530 GB), hardware floors, Mac Studio recipe. https://dev.meta.ai/resources/blog/build-with-muse-spark-open Verified 2026-10-16.

Meta (2026). *Muse-Spark-Open-816B-A42B* model card and Muse Model License. Hugging Face, vendor-reported benchmark table and licence terms. https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B Verified 2026-10-16.

Hugging Face (2026). *Welcome Muse Spark Open*. Hugging Face blog: architecture, key-value cache size, measured local speeds and hardware recipes. https://huggingface.co/blog/meta-muse-spark-open Verified 2026-10-16.

Unsloth (2026). *Muse-Spark-Open-816B-A42B-GGUF*. Hugging Face repository, file sizes summed per quantization. https://huggingface.co/unsloth/Muse-Spark-Open-816B-A42B-GGUF Read 2026-10-16.

OpenRouter (2026). *Meta: Muse Spark Open*. Model listing with per-host rates. https://openrouter.ai/meta/muse-spark-open Verified 2026-10-16.

Artificial Analysis (2026). *Muse Spark Open*, *Muse Spark 1.3*, *GLM-5.3*, *Kimi K3*, *Qwen3.8 2.4T A95B* and *DeepSeek V4.1 Flash* model pages, and *Open weights models* leaderboard. Independent benchmark index. https://artificialanalysis.ai/models/muse-spark-open and https://artificialanalysis.ai/models/open-source Read 2026-10-16.
