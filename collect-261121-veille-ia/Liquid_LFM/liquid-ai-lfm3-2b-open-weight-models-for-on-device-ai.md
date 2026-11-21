---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-lfm3-2b-open-weight-models-for-on-device-ai
title: "Liquid AI LFM3: 2B Open-Weight Models for On-Device AI"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["model", "liquid", "text", "models", "lfm3", "lfm3-1", "layers", "context"]
source: https://www.aighten.com/en/blog/liquid-ai-lfm3-open-weight-edge-models-benchmarks
source_anchor: ""
source_lines: [1, 89]
section: "Liquid_LFM"
---
Liquid AI released LFM3 on October 27, 2026: a family of open-weight models of roughly 2 billion parameters built to run on phones, laptops and other edge devices. The first release covers a 1.7B text model, a vision-language model, an audio model and an omni model that takes text, images, video and speech and can answer out loud, plus two decision models. All of them branch from one base model, LFM3-1.7B-Base (Liquid AI).

The headline claim is that a 1.73B-parameter text model beats several larger small models on tool calling and instruction following while using a fraction of their memory at long context. The weights are on Hugging Face under the LFM Open License v1.0, which is free for commercial use only below $10 million in annual revenue.

- LFM3 is a family of six models around 2B parameters sharing one backbone: text, vision, audio, omni and two decision variants, plus the base model.
- LFM3-1.7B has a 256K-token context window and takes about 1 GB of memory at 4-bit precision, per its model card.
- In Liquid AI's own tests, LFM3-1.7B averages 49.37 across ten benchmarks, ahead of Qwen3.5-9B (47.24) and Qwen3.5-4B (39.27), but it trails both on knowledge-heavy GPQA Diamond.
- A full 128K-token KV cache takes 1.6 GB, compared with 4.3 GB for Qwen3.5-4B and 5.6 GB for MiniCPM5-2B, according to Liquid AI.
- Companies with $10 million or more in annual revenue need a separate license for commercial use.
- Liquid AI says the 1.7B model is not meant for agentic coding or complex multi-step reasoning.

LFM stands for Liquid Foundation Models, Liquid AI's line of small models for local use. LFM3 is the third generation. Instead of training each modality separately, Liquid AI pre-trained one base model on 22 trillion tokens, extended its context to 256K tokens in the last 3.5 trillion, and then branched every specialist model from it. Each of the six models can be fine-tuned on a single GPU, the company says.

| Model | Parameters | Inputs | Outputs | Context | 
|---|---|---|---|---|
| LFM3-1.7B | 1.73B | Text | Text | 256K | 
| LFM3-VL-2B | 1.92B | Text, images, video | Text | 128K | 
| LFM3-Audio-2B | 2B | Text, audio | Text, speech | 32K | 
| LFM3-Omni-2B | 2.17B | Text, images, video with sound, audio | Text, speech | 32K | 
| LFM3-1.7B-Base | 1.73B | Text | Text | 256K | 

Sources: model cards for LFM3-1.7B, LFM3-VL-2B, LFM3-Audio-2B and LFM3-Omni-2B. The two decision models are specialized from the vision and omni models; Liquid AI's announcement gives no separate specs for them.

A few details from the model cards:

- LFM3-1.7B is post-trained for agent harnesses such as Hermes Agent, Pi and OpenClaw, and supports 16 languages including Korean and Japanese.
- LFM3-VL-2B is the first LFM vision model that accepts video, sampled at 2 frames per second. It can reason step by step before answering and is tuned for spatial reasoning, which Liquid AI pitches for robotics.
- LFM3-Audio-2B handles voice chat, transcription, speech synthesis and translation, and turns spoken requests into tool calls. Output speech is 24 kHz.
- LFM3-Omni-2B watches and listens at the same time, so it can answer questions about a video and its soundtrack together. It is a non-reasoning model.

LFM3 mixes three kinds of layers in its 24-layer backbone: 13 gated short convolution layers, 5 sliding-window attention layers that look only at the last 256 tokens, and 6 global attention layers that see the whole context. Liquid AI calls this a tri-hybrid design.

**Sliding-window attention** limits each layer to a fixed window of recent tokens, which keeps memory flat as the input grows. Because most layers are convolutions or sliding-window attention, only the six global attention layers store keys and values for the full context, which keeps the KV cache small at 128K tokens.

Other changes from LFM2 include a longer convolution kernel (7 instead of 3), head-wise gated attention, partial rotary position embeddings in the global layers and sandwich normalization. The vocabulary is 128,000 tokens. Liquid AI says it searched the architecture across 14 device and backend combinations, including phones, a Raspberry Pi 5, laptops, cloud CPUs, GPUs and the Qualcomm Hexagon NPU, and that LFM3-1.7B outperforms its LFM2.5-2.6B with 35% fewer parameters.

Liquid AI compared LFM3-1.7B with its own LFM2.5-2.6B and with MiniCPM5-2B, Gemma 4 E4B, Qwen3.5-4B and Qwen3.5-9B. These are vendor-reported numbers; no independent evaluation had been published at the time of writing.

| Benchmark | LFM3-1.7B | LFM2.5-2.6B | MiniCPM5-2B | Gemma 4 E4B | Qwen3.5-4B | Qwen3.5-9B | 
|---|---|---|---|---|---|---|
| Average | 49.37 | 31.55 | 39.19 | 31.74 | 39.27 | 47.24 | 
| BFCL V4 (function calling) | 71.39 | 55.89 | 62.65 | 48.25 | 55.12 | 62.83 | 
| IFBench (instruction following) | 73.03 | 60.48 | 62.08 | 44.56 | 38.16 | 48.84 | 
| SWE-bench Verified (OpenCode) | 43.40 | 18.00 | 34.80 | 0.50 | 41.20 | 51.20 | 
| WebArena-Verified | 38.42 | 3.02 | 9.11 | 17.06 | 21.92 | 28.94 | 
| GPQA Diamond | 63.37 | 57.78 | 65.15 | 57.88 | 75.76 | 80.40 | 
| RULER 128K | 65.56 | 1.15 | 37.96 | 18.82 | 84.63 | 87.11 | 

Source: Liquid AI LFM3 announcement.

LFM3-1.7B leads on function calling, instruction following and web-agent tasks, which is what it was post-trained for. It falls behind the Qwen models on GPQA Diamond, a graduate-level science quiz, and on RULER 128K, a long-context retrieval test, where Qwen3.5-4B scores 84.63 to its 65.56. Liquid AI itself says size matters most for coding and knowledge, and its model card recommends pairing the model with retrieval or search for knowledge-heavy tasks.

The SWE-bench Verified score of 43.40 is close to Qwen3.5-4B, yet the model card says LFM3-1.7B is "not recommended for agentic coding or complex multi-step reasoning due to its small size." Liquid AI points it at local agents, tool calling, structured outputs and fine-tuning instead.

Liquid AI reports these memory and speed results:

- On-device with llama.cpp at Q4_0 and 32K context, LFM3-1.7B decodes 1.8 to 2.8 times faster than the next-fastest model in its comparison and uses the least memory.
- The KV cache for a full 128K-token context is 1.6 GB, compared with 4.3 GB for Qwen3.5-4B and 5.6 GB for MiniCPM5-2B.
- On a single NVIDIA B300 with SGLang in BF16, the model serves about 3.6 billion tokens per day at 1,024 concurrent requests.

For local use, the number that matters most is the 1 GB footprint at 4-bit, which the model card says fits on any phone or laptop. If you want a larger local model for coding, our post on Recursive Flint, a 40B local coding model, covers the other end of the range.

The weights are in the LFM3 collection on Hugging Face, with ready-made variants:

- BF16 for vLLM, SGLang and Transformers on GPUs.
- FP8 and NVFP4 quantizations for SGLang.
- GGUF for llama.cpp, Ollama and LM Studio on CPUs and GPUs.
- MLX builds (4-bit to BF16) for Apple Silicon.
- ONNX for cross-platform runtimes.

Liquid AI also offers the models in its Playground and API, and in Pipette, its open-source model benchmarking tool. AMD, Ambarella and Qualcomm provided launch statements.

LFM3 ships under the LFM Open License v1.0, not Apache 2.0 or MIT. The license text on Hugging Face grants broad rights to use, modify and redistribute the models, but commercial use is licensed only if your company's annual revenue is under $10 million. Above that threshold you need a separate agreement with Liquid AI. Qualified non-profits doing non-commercial or research work are exempt from the threshold (LFM3-1.7B license).

Below the threshold, the terms work much like a permissive license. Larger companies should check with legal before shipping LFM3 in a product. Our post on Meta's Muse Spark open weights covers a different set of license terms for local models.

LFM3 makes the most sense when you need an agent or assistant to run locally with little memory: tool calling on a phone, a voice assistant on a laptop, document and screen reading on a device, or a vision model for a robot that cannot depend on the cloud. The omni model is interesting for anyone who currently chains a speech recognizer, a vision model and a text-to-speech engine, since one 2.17B model covers all three.

It makes less sense for coding agents, long multi-step reasoning or questions that depend on broad world knowledge, where larger models such as Qwen3.5-9B still score higher in Liquid AI's own table.

LFM3 is the third generation of Liquid AI's Liquid Foundation Models, released on October 27, 2026. It is a family of open-weight models of about 2B parameters for on-device use, covering text, vision, audio, omni (text, images, video and speech) and decision tasks, all branched from LFM3-1.7B-Base.

According to its model card, LFM3-1.7B takes about 1 GB of memory at 4-bit precision. Liquid AI says a full 128K-token KV cache adds 1.6 GB.

Only for organizations with annual revenue below $10 million. The LFM Open License v1.0 does not license commercial use by companies at or above that threshold; they need a separate agreement with Liquid AI.

It depends on the task. In Liquid AI's own benchmarks, LFM3-1.7B beats Qwen3.5-4B and Qwen3.5-9B on function calling (BFCL V4) and instruction following (IFBench), but scores lower on GPQA Diamond and on the RULER 128K long-context test. These are vendor-reported results.

Yes. Liquid AI designed LFM3 for edge devices and tested its architecture on phones, a Raspberry Pi 5, laptops and the Qualcomm Hexagon NPU. GGUF and MLX builds are available for local runtimes such as llama.cpp, Ollama, LM Studio and MLX.
