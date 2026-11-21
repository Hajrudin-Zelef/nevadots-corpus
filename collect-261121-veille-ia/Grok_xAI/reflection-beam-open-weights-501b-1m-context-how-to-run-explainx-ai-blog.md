---
id: collect-261121-veille-ia/grok-xai/reflection-beam-open-weights-501b-1m-context-how-to-run-explainx-ai-blog
title: "Reflection Beam Open Weights: 501B, 1M Context, How to Run | explainx.ai Blog"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-16"]
keywords: ["reflection", "beam", "weights", "model", "experts", "token", "context", "october"]
source: https://explainx.ai/blog/reflection-beam-501b-open-weights-released-hugging-face-apache-2026
source_anchor: ""
source_lines: [1, 146]
section: "Grok_xAI"
---
**Related:** this is the follow-up to our preview coverage, Reflection AI Beam: 501B open-weight MoE, weights due this month, and the FT report on Nvidia's talks to buy or deepen its stake in Reflection.

Reflection's **Beam** is now downloadable. On October 16, 2026, Hugging Face published "Welcome Beam: a 501B open model you can own, customize, and scale", announcing that the first model from the new US lab is on the Hub as reflection/Beam-1.0-501B-A23B. It is a mixture-of-experts model with 501 billion parameters, 23 billion active per token, text input, a 1M-token context window and an **Apache 2.0** license.

That closes the gap we flagged on October 5, when the weights were only promised. This post covers what shipped, the final benchmark table, what it takes to run, how to fine-tune it, and which claims remain the vendor's own.

## Update (October 16, 2026): AWS publishes a SageMaker deployment path for Beam

Later on October 16, AWS published Introducing Reflection Beam: an open-weight model for agentic coding on AWS on its machine learning blog. It adds a managed route to the weights.

- **SageMaker JumpStart:** "At launch, Beam is available in Amazon SageMaker JumpStart in US East (N. Virginia) and US East (Ohio)." The post does not mention Amazon Bedrock, and it gives no pricing beyond a reminder to delete test resources.
- **HyperPod Inference:** a second path that deploys through JumpStart, SGLang or vLLM configurations. The post names no supported regions and tells readers to pick a Region that offers the accelerated instance they need.
- **Instances:** FP8 runs on`ml.p5.48xlarge` (H100) or the`ml.p5e.48xlarge` and`ml.p5en.48xlarge` (H200) types. NVFP4, the default variant, runs on`ml.p6-b200.48xlarge` (B200). BF16 needs a multi-node setup.
- **Try it:** clone the`aws-samples` sagemaker-genai-hosting-examples repository and run`python jumpstart_real_time.py --variant fp8 --endpoint-name reflection-beam-fp8-smoke` . The script deploys an endpoint, sends a test request and cleans up.

AWS reports no benchmark scores of its own. It repeats Reflection's claim of a 1 million token effective context and advises teams to measure their own success rate, p50 and p95 time to first token, and output tokens per second. That matches the caveat above: the numbers in this post are still Reflection's.

## TL;DR: what shipped on October 16

| Question | Answer | 
|---|---|
| Is it out? | Yes: weights on Hugging Face, BF16, FP8 and NVFP4 checkpoints | 
| License | Apache 2.0 | 
| Size | 501B total, 23B active, 52 layers, 256 experts with 8 routed per token | 
| Context | 1M tokens, text in and text out | 
| Reasoning control | Five levels: low, medium (default), high, xhigh, max | 
| Day-0 inference | transformers (5.20.1 or newer), vLLM, SGLang | 
| Hosted options | Hugging Face Inference Providers (Baseten, Fireworks, Together) and Inference Endpoints | 
| Extras | Rust-based open-source harness, DFlash2 speculative drafter, TRL and Miles recipes | 
| Verified independently? | Not yet. Benchmark numbers are Reflection's | 

## What is inside the model?

Beam is built for coding and agentic work, and Hugging Face describes it as the model for teams that "want a frontier-scale model on their own hardware, infrastructure, or cloud." Its design has several unusual choices, all documented in the Hugging Face post.

Mixture-of-experts routing sends each token to a few experts, the idea behind Beam open weights

**Mixture of experts.** The first 3 layers are dense. The other 49 each hold 256 experts, with 8 chosen per token using the DeepSeek-V3 group-limited router (8 groups of 32, best 4 groups kept, then the top 8 experts inside them), and no shared expert. The experts hold 98.4% of the parameters. The consequence for deployment: all 256 experts stay in memory, so you need the memory of a 501B model but compute like a 23B one. For the buying decision, see our open-weight versus closed guide, and note the way Nvidia's Nemotron 3 Ultra made a similar trade.

Reflection Beam open weights architecture: three dense layers then 49 mixture-of-experts layers with hybrid attention
*Image: Hugging Face and Reflection*

**Hybrid attention for cheap long context.** Most layers use sliding-window attention over the previous 512 tokens. Every sixth layer, and the last, attends to the full context: 43 sliding-window layers and 9 global ones. Global layers use no positional encoding and instead scale queries as the visible context grows. The payoff is the KV cache. Hugging Face's table shows a full 1M-token sequence needing about 36 GiB per sequence in BF16 instead of 208 GiB if all 52 layers were global, and 128K tokens needing 4.6 GiB instead of 26 GiB.

**A constant-scale residual stream.** Instead of adding each sublayer output with weight 1, Beam adds the output at step k with weight 1 over the square root of k and shrinks the existing stream accordingly, so the hidden state keeps a constant scale across all 104 additions. Sublayers are normalized before and after, and the final logits pass through a tanh softcap at plus or minus 30. This is the sort of detail that matters when you fine-tune, because it explains why precision handling is fixed in specific places (router, norms, softcap in FP32).

## How do the benchmarks look?

The table below is copied from the Hugging Face post, which says the results were "provided by the Reflection team." The three columns are the three checkpoint formats, which is useful in itself: it shows what quantization costs.

| Benchmark | BF16 | FP8 | NVFP4 | 
|---|---|---|---|
| SWE-bench Verified | 81.6% | 80.2% | 80.7% | 
| SWE Bench Pro v1 | 66.1% | 65.9% | 65.6% | 
| SWE Bench Pro v2-Hard | 76.5% | 78.4% | 78.6% | 
| Terminal Bench v2.1 | 80.1% | 78.9% | 78.2% | 
| DeepSWE v1.1 | 43.3% | 43.0% | 42.7% | 
| SWEBench Multilingual | 78.3% | 78.1% | 77.5% | 
| AIME 2026 | 97.7% | 98.0% | 97.5% | 
| GPQA Diamond | 90.6% | 90.3% | 89.3% | 
| LiveCodeBench | 88.3% | 88.1% | 87.4% | 
| HLE (no tools) | 36.0% | 36.0% | 34.8% | 
| HLE (with tools) | 51.1% | 51.1% | 51.1% | 
| MCP Atlas | 80.8% | 81.5% | 80.5% | 
| BrowseComp with context management | 76.8% | 76.7% | 75.6% | 
| LongBench v2 | 67.5% | 67.6% | 67.2% | 
| IFBench | 79.6% | 80.7% | 79.2% | 

Two things stand out. First, the quantized checkpoints stay within about two points of BF16 almost everywhere, which supports Hugging Face's advice that NVFP4 is "the most practical way to serve the model." Second, the numbers are close to, but not identical with, the launch table from October 5 (for example DeepSWE v1.1 44.4 then, 43.3 now, and SWE-bench Verified 80.9 then, 81.6 now). That is a reminder that vendor tables shift between preview and release, so rerun your own tasks.

As we noted in the preview, Beam trails the newest Chinese open models such as Kimi K3 and GLM-5.3 on several coding benchmarks in Reflection's own comparison. Its pitch is efficiency per token and a US-origin Apache 2.0 license, not the top of every chart. Nothing here has been reproduced by a third party yet.

## What hardware do you need to run Beam?

This is the question most readers will ask next, and Hugging Face answers it directly.

| Checkpoint | Size | Fits on | 
|---|---|---|
| BF16 | about 1 TB | 16 H200s across two nodes (vLLM example) | 
| FP8 | roughly half | 8 H200s (SGLang example) | 
| NVFP4 | about 280 GB | 4 B300s; two B200s hold the weights but leave little room, so plan on four | 

Hugging Face says a B200 has 180 GB of HBM3e, so two (360 GB) can hold the weights with little room for the KV cache, and recommends four B200s (720 GB), a full 8xB200 node, two B300s at 288 GB each, or even 8xRTX Pro 6000. NVFP4 runs natively only on Blackwell. If you lack that hardware, the serverless route is simpler. Our coverage of Nvidia's RTX Spark launch ecosystem shows the other end of the local-AI spectrum, which Beam does not target.

## How do you serve it?

Install the current transformers (version 5.20.1 or newer) and use the text-generation pipeline:

```
from transformers import pipeline
pipe = pipeline(
    "text-generation", model="reflection/Beam-1.0-501B-A23B", device_map="auto"
)
messages = [{"role": "user", "content": "Write a limerick about Python exceptions."}]
output = pipe(messages, max_new_tokens=2000)
```
For production, vLLM and SGLang are the intended paths. The vLLM command from the post serves BF16 across 16 GPUs with an OpenAI-compatible API:

```
vllm serve reflection/Beam-1.0-501B-A23B --tensor-parallel-size 16 --served-model-name beam
```
Adding the DFlash2 speculative drafter, where a small model proposes up to 15 tokens ahead and Beam verifies them in one pass, costs only a little extra memory. SGLang serves the NVFP4 checkpoint on four B300s with a 1,048,576-token context, using the beam tool-call and reasoning parsers, and the FP8 checkpoint on eight H200s with a different attention backend. If you prefer not to manage servers, use Inference Providers through the router with the model name reflection/Beam-1.0-501B-A23B, or deploy from the Inference Endpoints catalog with `hf endpoints catalog deploy`.

Two practical details from the tokenizer: it ships a response template, so `parse_response` splits reasoning, the answer and tool calls without hand-written regex, and tool arguments use an XML-style grammar that the parser casts to your declared types when you pass the tool schemas. The chat template also warns that dropping earlier reasoning degrades output, which is why the `preserve_thinking` behavior matters for multi-turn agents.

## What did Hugging Face's own tests show?

Hugging Face ran what it calls "vibe evals, not benchmarks" on five tasks at low and high reasoning effort through Reflection's platform: multi-step tool calling, long-context retrieval over a codebase, a terminal repair task, an AIME-style counting problem, and strict structured output. High effort passed all five. At low effort, three passed cleanly, one answered the math question correctly but omitted the requested justification, and one long-context response had the right code behavior and citations but did not match the requested format, which the authors say is partly prompt ambiguity. Token use roughly doubled to quintupled between low and high. The full traces are in a public bucket, so you can inspect them yourself.

The takeaway is practical: reasoning effort is a real cost dial. Medium is the default, and for latency-sensitive agent steps you will want to test low before paying for xhigh or max.

## Can you fine-tune it?

Yes, and this is where open weights earn their keep. The post includes two recipes.

A LoRA GRPO run on Beam raising AIME 2025 accuracy from 42.9 to 64.2 percent with shorter outputs
*Image: Hugging Face and Reflection*

**SFT with TRL.** LoRA adapters trained on Multilingual-Thinking fit on a single node with eight B300 GPUs at roughly 19,000 tokens per second (the H100 equivalent uses four nodes). After one epoch Beam reasoned in the requested language on 16 of 16 held-out prompts versus 3 of 16 for the base model. The authors stress that this measures language adherence, not reasoning correctness.

**GRPO with Miles.** Reflection and RadixArk built a recipe using Megatron for training and SGLang for rollouts. The reference setups are 32 H200s for LoRA GRPO and 96 H200s for full-parameter GRPO. In the LoRA run on DAPO-Math-17K, accuracy on AIME 2025 rose from 42.9% to 64.2% while mean output length fell from 3,188 to 2,197 tokens, per the post's chart. Full-parameter training moved a 4,000-token-limited eval from 46.25% to 55.8%. These are single runs on a small task, not a general promise, but they show the model is trainable by outsiders at a cost that is large yet finite. For background on the RL tooling, see our guide to Hugging Face Hub RL environments.

## Why this release matters

For a year, the practical open-weight frontier has been mostly Chinese (DeepSeek, Qwen, Kimi, GLM). Beam is a frontier-scale, Apache 2.0, US-origin option with serious tooling around it. That matters for regulated buyers and for the broader American closed models versus Chinese open weights debate. It also lands the same month as other open-weight moves, such as Meta's open Muse Spark and Nvidia's work with a Muse Spark DFlash draft model, which shows speculative decoding becoming standard for big sparse models.

Corporate context is also moving. The FT reported Nvidia is in talks to acquire or deepen its stake in Reflection; we cover what is and is not confirmed in our Nvidia and Reflection post. Nvidia hardware is central to the release: the NVFP4 checkpoint is built for Blackwell.

## Caveats before you build on it

- **Vendor benchmarks.** Every score above comes from Reflection through Hugging Face. Wait for independent runs, and build your own 30 to 100 task evaluation set.
- **Memory, not compute, is the bill.** Even at NVFP4 you need roughly 280 GB of fast memory before the KV cache. Self-hosting only pays off at sustained volume.
- **Text only.** Beam takes no image input.
- **Read the license file.** Apache 2.0 is permissive, but confirm the terms on the model repo, and check that any hosted provider you use honors them.
- **Young tooling.** Day-0 support is real, but expect fixes in vLLM, SGLang and the parsers over the coming weeks.
- **Prize program.** Reflection is offering $10,000 in Hub credits to the best projects built with Beam (demos, quantizations, datasets, RL environments, benchmarks), which should accelerate community builds.

## What this means for what you build or pay

If you run coding agents at scale and currently pay per token for a hosted open model, Beam is worth a one-week bake-off against it using the Inference Providers route, because it needs no hardware. If data cannot leave your network, plan a four-B200 or eight-H200 deployment and budget for the memory first. If you only run models on a laptop or single GPU, Beam is not for you yet; wait for community quantizations. In all cases, keep a closed-model fallback for the hardest tasks, as our open-weight versus closed guide recommends.

## Related reading

**Primary sources:** Hugging Face: Welcome Beam · Beam-1.0-501B-A23B model repo · Hacker News discussion

*Details are accurate as of October 16, 2026 and come from Hugging Face's launch post and model card. Benchmark results are reported by Reflection and not independently verified.*
