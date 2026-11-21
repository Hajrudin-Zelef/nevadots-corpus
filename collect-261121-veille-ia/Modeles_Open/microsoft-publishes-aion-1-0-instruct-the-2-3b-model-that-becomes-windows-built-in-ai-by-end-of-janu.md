---
id: collect-261121-veille-ia/modeles-open/microsoft-publishes-aion-1-0-instruct-the-2-3b-model-that-becomes-windows-built-in-ai-by-end-of-janu
title: "Microsoft publishes Aion 1.0 Instruct, the 2.3B model that becomes Windows' built-in AI by end of January 2027"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "microsoft", "windows", "aion", "instruct", "built-in", "january", "says"]
source: https://onpremworld.com/en/news/2026-11-microsoft-aion-1-0-instruct-open-weights
source_anchor: ""
source_lines: [1, 40]
section: "Modeles_Open"
---
Models Published:

# Microsoft publishes Aion 1.0 Instruct, the 2.3B model that becomes Windows' built-in AI by end of January 2027

## What happened

Microsoft's Windows Applied Sciences Group published Aion 1.0 Instruct on Hugging Face on November 18 under the MIT license. It is a 2.3-billion-parameter instruction-tuned text model with a 32,768-token context, tool calling and eight listed languages, including Chinese and Japanese, built for summarization, rewriting and chat on the device. Microsoft says a further optimized version, paired with its content moderation model, will become the Windows OS small language model by the end of January 2027, succeeding Phi Silica. The unquantized bfloat16 weights came first; an INT4 ONNX WebGPU build of about 1.3 GB is already up, and Microsoft says more quantized versions will follow.

**Why it matters locally**: The model that Windows itself will run on the device is now a free download under MIT, so you can test what the built-in AI can and cannot do on your own documents before it ships.

## Three ways to read it

### For the market

Microsoft is publishing its operating-system model in the open before it ships, which it describes as a way to maximize transparency and invite feedback. In our view this puts a permissively licensed small model from the Windows team next to Google's Gemma and Meta's small Llama models, and it could make a 2B-class model the baseline every Windows app developer assumes is present.

### In plain words

Microsoft has shared the small AI that will soon be built into Windows to summarize and rewrite text without sending it to the internet. Microsoft itself says a model this small can miss details and get facts wrong, so think of it as a quick helper for tidying text, not an expert.

### For practitioners

The bfloat16 safetensors file is about 4.6 GB and the INT4 ONNX WebGPU build is about 1.3 GB; the Transformers path needs trust_remote_code=True because of custom modeling code, and the card says it was exported with transformers 5.x. The architecture is 28 layers with 21 sliding-window layers (window 2048) and 7 full-attention layers, and the knowledge cutoff is 2025-12. Microsoft reports MMLU 55.15 and IFEval 83.90, says multilingual data is about 6% of training data and that the model was not trained to excel at world knowledge, so test it on grounded tasks such as summarizing your own text, and check Chinese output quality yourself.

## Subscribe to Sprout's weekly

One email a week: the top stories and a taste of Sprout’s insights. Unsubscribe any time.

## Source

Read the official source: microsoft/Aion-1.0-Instruct-2.3B model card (Hugging Face) ↗
Summaries are written by OnPrem World from the official source. We link to the original and never republish it.

## More news

- NVIDIA and Microsoft bring Nemotron 3.5 and RTX PRO 6000 Blackwell support to Azure Local
- RTX Spark compact desktops from ASUS, Dell and MSI go on sale with up to 128 GB of unified memory
- NVIDIA reports $115.8 billion quarter; highlights include a deal to acquire Hugging Face and a 64GB DGX Spark
- Open WebUI v0.13.4 moves personal connections into Tether, sandboxes agents on your own computer and runs local MCP servers
- vLLM v0.34 sizes the KV cache from real free memory, removes Model Runner V1 and adds Mistral Large 4
