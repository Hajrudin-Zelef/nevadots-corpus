---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-lfm3-six-2b-models-licence-and-hardware-needs
title: "Liquid AI LFM3: Six 2B Models, Licence and Hardware Needs"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-10-28"]
keywords: ["liquid", "models", "licence", "model", "liquids", "text", "lfm3", "lfm3-1"]
source: https://benchclaw.io/lfm3/
source_anchor: ""
source_lines: [1, 140]
section: "Liquid_LFM"
---
**LFM3 is Liquid AI’s new family of six open-weight models, all about 2B parameters, and its licence is free for commercial use only below $10 million in annual revenue.** Liquid announced it on 27 October 2026. We read Liquid’s blog post, its model pages, its licence guide and the Hugging Face records on 2026-10-28, and we ran nothing. Every benchmark below is Liquid’s own, and the memory figures are file sizes Hugging Face lists or our arithmetic on them.

Our methodology says how we treat evidence. Liquid’s text model is small enough for any 8 GB graphics card, and its published scores are strong on tool use and instruction following and weak on knowledge, coding and long context, so the average hides a split.

## What is LFM3?

LFM3 is the third generation of Liquid Foundation Models. Six models share one backbone, LFM3-1.7B-Base, pre-trained on 22 trillion tokens. Liquid says the design mixes three layer types: of 24 layers, 13 are gated short convolutions, 5 are sliding-window attention over 256 tokens, and 6 are global attention. The text, vision, audio and omni models, plus two decision models, are all post-trained from that base. The base and post-trained weights are open on Hugging Face, and Liquid also offers an API.

## Which LFM3 models exist?

| Model | Parameters | What it does (Liquid’s description) | Context | 16-bit weights on Hugging Face | 
|---|---|---|---|---|
| LFM3-1.7B | 1.73B | Text, for local agents, native tool calling | 256K tokens | 3.46 GB | 
| LFM3-VL-2B | 1.92B | Text, images and video in; first LFM vision model with video | 128K tokens | 3.84 GB | 
| LFM3-Audio-2B | 2.00B | Speech recognition, speech synthesis, voice chat | 32K tokens | 4.15 GB | 
| LFM3-Omni-2B | 2.17B | Vision, audio and text in one model | not read | 4.48 GB | 
| d1-2B | 1.92B | Decision model over text, pictures and video | not read | 3.84 GB | 
| d1-Omni-2B | 2.17B | The same decision model with audio input | not read | 4.33 GB | 

A decision model returns a calibrated answer in one forward pass without generating tokens, and Liquid quotes 5.7 ms per decision on an NVIDIA H100. We did not test any of it.

## What does the licence say?

All six use the LFM Open License v1.0, which Liquid says is based on Apache 2.0. The difference is a commercial-use threshold. Liquid’s guide says commercial use is free for a company with annual revenue under $10 million, and a company at $10 million or more needs a separate commercial licence from Liquid. Revenue counts for the whole legal entity and the companies it controls or shares control with. Modified and fine-tuned models carry the same threshold, but you do not have to open-source your changes and you may license them under your own terms. Research use is allowed, and qualified nonprofits are exempt for non-commercial or research use.

Two details matter. Liquid says using a model in your business can count as commercial use even if the model does not directly earn money, while internal research, testing and evaluation are allowed without a commercial licence. And the guide says the licence ends automatically if you break any of its terms. These are Liquid’s own plain-language summaries, not legal advice, so read the licence text before you build on it. Standard Apache 2.0 has no revenue limit, which is why we do not count this licence as the same thing.

## How good are the benchmarks?

Liquid’s numbers, from its own blog post and model cards, are the only benchmarks we have. Its text table scores LFM3-1.7B against five models on 29 tests. The average is 49.37 for LFM3-1.7B and 47.24 for Qwen3.5-9B, which has 9.7B parameters in Liquid’s table. We re-added the 29 rows and both averages reproduce. Counting the rows ourselves, LFM3-1.7B is ahead on 14 and Qwen3.5-9B is ahead on 15.

| Where LFM3-1.7B is ahead of Qwen3.5-9B | LFM3-1.7B | Qwen3.5-9B | 
|---|---|---|
| AA-Omniscience non-hallucination | 92.93 | 52.59 | 
| AutomationBench (public) | 41.29 | 15.10 | 
| IFBench | 73.03 | 48.84 | 
| WebArena-Verified | 38.42 | 28.94 | 
| BFCL V4 (function calling) | 71.39 | 62.83 | 

| Where Qwen3.5-9B is ahead | LFM3-1.7B | Qwen3.5-9B | 
|---|---|---|
| RULER 128K (long context) | 65.56 | 87.11 | 
| GPQA Diamond | 63.37 | 80.40 | 
| SWE-bench Multilingual (OpenCode) | 23.00 | 35.00 | 
| SWE-bench Verified (OpenCode) | 43.40 | 51.20 | 
| MCP-Atlas | 32.57 | 42.67 | 

The average edge comes from a few large gaps, mainly the two AA-Omniscience rows and AutomationBench. Liquid says as much: larger models still win on agentic coding and knowledge, and its model page says the text model is not recommended for agentic coding or complex reasoning. Liquid ran these tests itself, and for rankings that disagree by who runs them, see why LLM leaderboards disagree and LLM evals vs benchmarks.

For vision, the LFM3-VL-2B card says it matches Gemma 4 12B, is within 2 points of GPT-5.6 Luna and scores higher than Gemini 3.5 Flash Lite, across 62 vision benchmarks. The card’s own average row agrees: 71.4 for LFM3-VL-2B, 71.7 for Gemma 4 12B, 73.4 for GPT-5.6 Luna and 70.0 for Gemini 3.5 Flash Lite. But Qwen3.5 4B also scores 73.4 in that table, above LFM3-VL-2B, and the card’s line that it scores “at least 8 points ahead of all models of similar size” does not hold against Qwen3.5 2B, which is 7.4 points behind. We read no audio or omni benchmark table.

## What do you need to run LFM3?

Liquid publishes no minimum-specification page for LFM3. It says the text model takes about 1 GB at 4-bit, fits any phone or laptop, and that a full 128K-token context needs 1.6 GB of cache for the text model. Beyond that, the sizes below are the files Hugging Face lists, and the sums are our arithmetic.

| Model | 4-bit file | 8-bit file | 16-bit weights | 
|---|---|---|---|
| LFM3-1.7B | 1.05 GB (Q4_0) | 1.85 GB (Q8_0) | 3.47 GB (GGUF) | 
| LFM3-VL-2B, with its vision projector | 1.30 GB (Q4_K_M plus 8-bit projector) | 2.05 GB (Q8_0 plus 8-bit projector) | 3.85 GB (BF16 plus BF16 projector) | 
| LFM3-Audio-2B, with its audio projector | 1.44 GB (Q4_K_M plus 8-bit projector) | 2.19 GB (Q8_0 plus 8-bit projector) | 4.04 GB (BF16 plus BF16 projector) | 
| LFM3-Omni-2B, with its projector | 1.64 GB (Q4_K_M plus 8-bit projector) | 2.39 GB (Q8_0 plus 8-bit projector) | 4.42 GB (BF16 plus BF16 projector) | 

These are weights only. The context cache and the runtime come on top: for the text model at full 128K context, 16-bit weights plus Liquid’s 1.6 GB cache figure come to about 5.1 GB, which is our sum and not a measurement.

**Graphics cards.** NVIDIA’s product pages list these memory sizes. The last column subtracts the largest 16-bit checkpoint, Omni at 4.48 GB, to show what is left for context and runtime:

| Card | Memory (NVIDIA) | Left after the largest 16-bit checkpoint | 
|---|---|---|
| GeForce RTX 5060 | 8 GB GDDR7 | about 3.5 GB | 
| GeForce RTX 5070 | 12 GB GDDR7 | about 7.5 GB | 
| GeForce RTX 5070 Ti and RTX 5080 | 16 GB GDDR7 | about 11.5 GB | 
| GeForce RTX 4090 | 24 GB GDDR6X | about 19.5 GB | 
| GeForce RTX 5090 | 32 GB GDDR7 | about 27.5 GB | 

Every checkpoint in the family fits an 8 GB card by file size, so memory is not the limit for these models. We tested no card and measured no speed. Liquid’s own speed figures come from a data-centre GPU, an NVIDIA B300 (3.6 billion tokens a day for the text model at 1,024 concurrent requests), which says nothing about a consumer card.

**CPUs and phones.** Liquid names a Samsung Galaxy S26 Ultra, a Ryzen AI Max+ 395 and an Apple M5 Max in its vision model’s edge test, and a Raspberry Pi 5 among the devices used to tune the architecture. We found no memory or speed figure for a Raspberry Pi or any minimum RAM figure beyond the 1 GB at 4-bit.

**Software.** The pages we read give these requirements:

- **Transformers:** version 5.21.0 or newer for the vision model and 5.20.2 or newer for the audio model. The examples load the models with`trust_remote_code=True` , which runs Python code from the model repository, so read that code before you use it.
- **llama.cpp, vLLM and SGLang:** each needs a build with LFM3 support, which Liquid’s pages state; SGLang 0.5.10 or newer for the text model. Liquid’s vision-model tests used vLLM 0.27.1 and SGLang 0.5.22.
- **Ollama:** Liquid’s Ollama page still describes LFM2.5, and we found no statement that Ollama runs LFM3.
- **Formats:** GGUF, MLX (Apple silicon), ONNX, plus FP8 and NVFP4 checkpoints for SGLang on GPUs. The audio model’s docs page says GGUF is “coming soon”, although a GGUF repository for it exists on Hugging Face, so check before you rely on it.
- **Fine-tuning:** Liquid says every model can be fine-tuned on a single GPU and recommends LoRA for a first pass. It publishes no memory figure for LFM3 fine-tuning, so we give none.

## Should you use LFM3?

This is our reading, not a measured result. If you want a small model on a phone or laptop for tool calls, structured output and instruction following, Liquid’s numbers say it is worth testing, and its 4-bit file is about 1 GB. If your task is knowledge, coding or long-context retrieval, Liquid’s own table points to larger models. If you sell a product on top of it, check the $10 million threshold first. Test on your own tasks either way, and put controls around any model that calls tools; see AI agent security and AI guardrails. For memory tables and graphics-card sizes for other open models, see our uncensored models article, and for the wider picture see agentic AI frameworks.

## What we did not check

- **Any model.** We downloaded and ran nothing, so we have no result on quality, speed, memory use or safety.
- **The benchmarks.** They are Liquid’s, from its blog and model cards. We re-did the arithmetic on the text table and read the vision average row, but did not rerun a test or read Pipette.
- **Audio, omni and decision model results,** and the context length of the omni and decision models.
- **Real hardware requirements.** Memory sums are arithmetic from listed file sizes plus Liquid’s cache figure, with no context, runtime or speed measurement.
- **The licence text.** We read Liquid’s licence guide and summary, not the full legal text, and make no legal claim.

## Sources, read 2026-10-28

- Liquid AI, Introducing LFM3: Every Modality for Specialized Intelligence (27 October 2026)
- Liquid docs, LFM3-1.7B, LFM3-VL-2B, LFM3-Audio-2B, fine-tuning overview and Ollama
- Liquid AI, LFM Open License user guide and LFM licence summary
- Hugging Face model cards and file listings for LFM3-VL-2B, d1-2B and the other LiquidAI LFM3 repositories
- NVIDIA GeForce product pages for the RTX 5090, 5080, 5070 family, 5060 family and 4090, read 2026-10-27

## FAQ

### What is LFM3?

LFM3 is Liquid AI’s third generation of Liquid Foundation Models, announced on 27 October 2026. It is six open-weight models of about 2B parameters, for text, vision, audio, omni and decision tasks, all built on one shared base. They are designed to run on devices such as phones and laptops.

### Is LFM3 free for commercial use?

Only below a revenue limit. Liquid’s licence guide says commercial use is free for a company with annual revenue under $10 million, and a company at $10 million or more needs a separate commercial licence. Research use is allowed. Read the full licence before building on it.

### What graphics card do I need for LFM3?

By file size, any 8 GB card can hold every LFM3 checkpoint: the largest, the omni model, is 4.48 GB in 16-bit, and the 4-bit text model is about 1 GB. We tested no card and measured no speed, so leave room for the context cache and the runtime.

### Can LFM3 run on a phone or a Raspberry Pi?

Liquid says the text model fits any phone or laptop at 4-bit, about 1 GB, and tested its vision model on a Galaxy S26 Ultra. It lists a Raspberry Pi 5 among devices used to tune the design but gives no memory or speed figure for it. We ran nothing on either.

### Is LFM3-1.7B better than Qwen3.5-9B?

Liquid’s own table is split. LFM3-1.7B averages 49.37 against 47.24 across 29 tests, but it is ahead on 14 and behind on 15. It leads on tool use and instruction following and trails on knowledge, coding and long context. These are Liquid’s results, and we ran none of them.

### Does LFM3 work with Ollama?

We found no statement that it does. Liquid’s Ollama page still describes LFM2.5 models. For LFM3, Liquid’s pages name llama.cpp, vLLM, SGLang and Transformers builds with LFM3 support, plus MLX and ONNX. Check the current support notes for the tool you plan to use before you commit.

## Related reading

- Uncensored models, with memory tables and graphics-card sizes
- AI agent security
- AI guardrails
- LLM evals vs benchmarks
- Agentic AI frameworks, the pillar for this cluster
- Open weight models in 2026: 14 official models, their licences and memory needs
