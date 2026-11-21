---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-releases-lfm3-family-of-six-2b-open-weight-models-built-for-edge-devices
title: "Liquid AI Releases LFM3 Family of Six 2B Open-Weight Models Built for Edge Devices"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["models", "liquid", "text", "model", "lfm3", "window", "family", "audio"]
source: https://techiexpert.com/liquid-ai-releases-lfm3-family-of-six-2b-open-weight-models-built-for-edge-devices/
source_anchor: ""
source_lines: [1, 70]
section: "Liquid_LFM"
---
**Key Takeaways:**

- Liquid AI has launched LFM3, a family of six open weight 2B-scale models spanning text, vision, audio, omni, and structured decision tasks.
- The models use an architecture optimized on 14 hardware devices, including Galaxy S26 Ultra phones, Raspberry Pi 5 boards, and Apple M4 Max laptops.
- LFM3 outperforms many models such as Qwen3.5-9B on agentic tool benchmarks and matches Gemma 4 12B on vision tasks.

Liquid AI has released LFM3, introducing its family of six open-weight foundation models around the 2 billion parameters mark designed to run and agentic tasks on local devices.

Introducing : our new generation of Liquid Foundation Models. With a new tri-hybrid architecture and new multimodal encoders, the LFM3 family is the ideal model suite for specialized intelligence.

> 6 model instances, across text, audio, vision, and omni modalities, generative and decision models.

 > -.: text, 256k context window.

 > –: vision-language model with video-input support for spatial understanding

 > –: tool calls, voice chat, reasoning in 18 languages.

 > –: an omni model combines vision, audio, and language.

 > – and –: decision models with typed outputs and calibrated probabilities in a single pass. Omni adds audio input.

 > fine-tune on 1 GPU.

 > a new tri-hybrid architecture

 > architecture optimized for 14 processor classes across GPUs, CPUs, and NPUs.

 > — is a beast. It performs competitively with Gemma 4 12B (6x larger), and Gemini 3.5 Flash Lite, and GPT-5.6 Luna, with 1.9B parameters.

 > -. is #1 on the Pareto frontier of device intelligence ..

Every modality for specialized intelligence. Pick the one that fits your use case, customize it, and run it on-device.

1/

— Liquid AI (@liquidai) October 27, 2026


Table of Contents

All six models branch out from a single LFM3-1.7B-Base backbone. To make the models fast and light, Liquid AI built a new architecture tested through 14 different processors and devices. This hardware list includes phones such as the Galaxy S26 Ultra and OnePlus 15, the Raspberry Pi 5, laptops running Apple M4 Max and AMD Ryzen AI chips, Arm and x86 CPUs, GPUs, and Qualcomm Hexagon NPUs.

**Read Next: ViggleAI Launches JST-2 to Build 3D Worlds and Characters With Built-In Physics** 

Inside the 24-layer network, LFM3 mixes 13 gated short convolution layers (using a larger kernel size of 7, up from 3 in LFM2), five sliding window attention layers with a 256 token window, and six global attention layers. The team added gated attention to make the models easier to compress, Partial RoPE on global attention layers, sandwich normalization, and a 128K token vocabulary.

Liquid AI pre-trained the base model on 22 trillion tokens with extra focus on scientific papers, full code repositories, and math. The final 3.5 trillion tokens of training focused on AI agent workflows and tool calls while stretching the context window up to 256K tokens.

The standard LFM3-1.7B text model features 1.73 billion parameters and a 256K context window. It takes up only about 1 GB of memory at 4-bit quantization and is tuned for local AI agents like Hermes Agent, Pi, and OpenClaw.

For visual tasks, LFM3-VL-2B combines the text model with a lightweight vision encoder that uses one third the computing power of SigLIP2. It supports a 128K context window, step by step reasoning, and video inputs at two frames per second.

LFM3-Audio-2B supports 18 languages for both text and speech across a 32K context window, using a fast encoder to cut down response wait times. LFM3-Omni-2B is Liquid AI’s first omni model, combining text, image, video, and audio understanding with speech output inside a 2.17 billion parameters model.

Rounding out the lineup are two decision models: d1-2B for text, images, and video, and d1-Omni-2B, which adds support for spoken audio questions.

Unlike chatbots that write out paragraphs of text, these decision models answer multiple questions in a pass and return structured data with confidence scores. They are built to output yes or no answers, multiple choice picks, bounding boxes on images, specific dates or numbers, and estimated number ranges for factory or business workflows.

According to Liquid AI’s internal testing, LFM3-1.7B scores an average of 49.37 across text benchmarks, beating the Qwen3.5-9B model (47.24) in several tests. It shines brightest at using tools and running agent tasks, scoring 96.66 on IFEval, 80.36 on ToolSandbox, and 71.39 on BFCL V4. On local hardware, it decodes text 1.8 to 2.8 times faster than rival models and needs only 1.6 GB of KV cache memory at 128K context, compared to 4.3 GB for Qwen3.5-4B. On a single NVIDIA B300 server GPU, it can process about 3.6 billion tokens per day.

In vision tests, LFM3-VL-2B averages 71.4 across 62 benchmarks, matching Google’s Gemma 4 12B, a model six times its size, with strong results in reading documents and navigating computer screens. LFM3-Omni-2B beats Qwen2.5-Omni-7B on the Daily Omni benchmark with a score of 70.1, the new d1-2B decision model beats Liquid AI’s older d1-3B model despite being a third smaller.

Because of their small 2B size, the models have clear limits. They trail larger models on deep trivia tests such as GPQA Diamond and complex coding. Liquid AI recommends pairing the models with search tools and avoiding them for heavy coding or complex math.

Developers can fine tune the LFM3 models on a single GPU. Fine tuning LFM3-VL-2B for only a few hours on one NVIDIA A100 GPU to spot wind turbine damage beat both the base model and OpenAI’s GPT-6 Luna. On edge hardware such as the NVIDIA Jetson AGX Orin, the models can drive robotics actions in under 50 milliseconds.

The LFM3 family works out of the box with popular tools including llama.cpp, MLX, vLLM, SGLang, ONNX, Ollama, and LM Studio through 4-bit, FP8, and NVFP4 formats. All six models are available under the LFM Open License v1.0, which allows free commercial use for companies making under $10 million in annual revenue.

**Read Next: NVIDIA Releases Isaac Lab 3.0 With a New Multi-Backend Design for Robot Training**
