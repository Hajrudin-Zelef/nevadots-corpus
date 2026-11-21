---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-aion-1-0-instruct-2-3b-what-hardware-runs-it
title: "Microsoft Aion 1.0 Instruct 2.3B: What Hardware Runs It?"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "aion", "instruct", "runs", "card", "windows", "about"]
source: https://modelfit.io/blog/microsoft-aion-1-0-instruct-2-3b/
source_anchor: ""
source_lines: [1, 98]
section: "Copilot_Microsoft"
---
Microsoft published **Aion 1.0 Instruct** on Hugging Face on 18 November 2026: `microsoft/Aion-1.0-Instruct-2.3B`, from the Windows Applied Sciences Group, under the MIT license (microsoft/Aion-1.0-Instruct-2.3B, 2026). The Hugging Face API counts 2,288,831,488 parameters in BF16, in a single `model.safetensors` file of 4,577,698,688 bytes, about **4.58 GB**. A second repo holds an INT4 build for WebGPU of about **1.3 GB** (microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU, 2026). The catch for local users: llama.cpp and mlx-lm do not support its architecture, so Ollama, LM Studio and MLX cannot load it today.

## What is Aion 1.0 Instruct?

Microsoft's model card calls it the foundational small language model of the Windows ecosystem, built for on-device text use. It also says that a further optimized version, paired with Microsoft's content moderation model, "will become the Windows OS SLM by the end of January 2027". Its training data extends the data used for Phi Silica, the previous Windows small model. Microsoft publishes the unquantized model first and says quantized versions will follow.

From the model card:

| Spec | Value | 
|---|---|
| Parameters | about 2.29B (tied input and output embeddings) | 
| Layers | 28, hidden size 2048 | 
| Attention | GQA, 24 query heads and 8 key/value heads | 
| Attention pattern | 21 sliding-window layers (window 2048) and 7 full-attention layers | 
| Context | 32,768 tokens | 
| Vocabulary | 200,029 tokens | 
| Languages | English, German, Spanish, Portuguese, Italian, French, Chinese, Japanese | 
| Precision | bfloat16 | 
| Knowledge cutoff | 2025-12 | 
| License | MIT | 

## Get told when a better model fits your MacBook Air M5 16 GB

One email when a new open-weight model is a better fit for your machine, plus the Thursday weekly.

For: **MacBook Air M5 16 GB**

You'll also get the weekly ModelFit email. By signing up you agree to our Privacy Policy. Unsubscribe from either email anytime, in one click.

It is trained for summarization, rewriting, document question answering and chat, and the chat template supports tool calls. The card warns that it "was not trained to excel at world knowledge".

## How does Aion 1.0 Instruct score?

These are Microsoft's own scores from the model card, run with EleutherAI's lm-evaluation-harness. ModelFit has not reproduced them.

| Benchmark | Score | 
|---|---|
| MMLU | 55.15 | 
| MMLU-Pro | 33.35 | 
| GSM8K | 74.91 | 
| MATH-500 | 48.00 | 
| IFEval | 83.90 | 
| Multi-IF | 64.70 | 

The strongest figure is instruction following, which fits a model meant to rewrite and summarize inside an operating system. Knowledge scores are modest, as Microsoft itself says.

## Which hardware can hold Aion 1.0 Instruct?

Almost anything recent. The weights are 4.58 GB in BF16 and about 1.3 GB in the INT4 build (the `model.onnx.data` file is 1,291,678,720 bytes). Both fit in the memory of an 8 GB machine, Mac or PC, with room left for the system. Microsoft gives no figure for memory in use while it runs, and we do not estimate one: the KV cache comes on top and grows with the context you load. The ONNX build keeps that cache in FP16, and its default `max_length` is 4,096 tokens, which you can raise to the full 32,768.

## How do you run it today?

Two official paths, both outside the usual local tools:

- **Transformers:** the model uses custom code, so it needs`trust_remote_code=True` . Microsoft exported it with transformers 5.x.
- **ONNX Runtime GenAI with WebGPU:** the INT4 build runs on a GPU through WebGPU, natively or in a browser with ONNX Runtime Web. The card installs`onnxruntime-webgpu` 1.27.0 and`onnxruntime-genai` 0.14.1. Its linear layers are 4-bit block-wise weights from a quantization-aware-trained checkpoint. Without the WebGPU provider, it runs on CPU.

## Will llama.cpp, Ollama and MLX run it?

Not yet. The architecture is `aion_instruct` (`AionInstructForCausalLM`), and on 19 November 2026 we found no pull request or issue for it in llama.cpp or mlx-lm. Until one of them supports it upstream, the tools built on them (Ollama, LM Studio, MLX) cannot load Aion either. For that reason ModelFit has no model page for it yet. We will add one when support lands.

## FAQ

### Is Aion 1.0 Instruct open weights?

Yes. The weights are on Hugging Face under the MIT license, not gated.

### How much memory does Aion 1.0 Instruct need?

The weights take 4.58 GB in BF16 and about 1.3 GB in INT4. Microsoft gives no runtime memory figure; the KV cache adds to the weights and grows with context.

### Can a Mac run Aion 1.0 Instruct?

The weights fit any recent Mac, but there is no MLX or llama.cpp support yet, so the only paths are Transformers or ONNX Runtime with WebGPU.

### Is Aion the model inside Windows?

Not this exact version. Microsoft says a further optimized version, with a content moderation model, becomes the Windows OS small model by the end of January 2027.

## Sources

- microsoft/Aion-1.0-Instruct-2.3B model card, config and file list: https://huggingface.co/microsoft/Aion-1.0-Instruct-2.3B
- Hugging Face API, parameter count and file sizes: https://huggingface.co/api/models/microsoft/Aion-1.0-Instruct-2.3B?blobs=true
- microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU model card and file list: https://huggingface.co/microsoft/Aion-1.0-Instruct-2.3B-ONNX-WebGPU
- llama.cpp: https://github.com/ggml-org/llama.cpp
- mlx-lm: https://github.com/ml-explore/mlx-lm

## Don't miss the next one for your MacBook Air M5 16 GB

This article covers one moment. Get one email when a newer open-weight model fits your machine better, plus the Thursday weekly with what changed.

For: **MacBook Air M5 16 GB**

You'll also get the weekly ModelFit email. By signing up you agree to our Privacy Policy. Unsubscribe from either email anytime, in one click.

Match this model to a machine that can run it: by RAM tier for Apple Silicon, or by VRAM for an NVIDIA GPU.

Have questions? Reach out on X/Twitter
