---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-launches-lfm3-family-of-six-open-weight-2b-class-models
title: "Liquid AI Launches LFM3 Family of Six Open-Weight 2B-Class Models"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["text", "models", "liquid", "model", "lfm3", "lfm3-1", "audio", "backbone"]
source: https://www.unite.ai/liquid-ai-launches-lfm3-family-of-six-open-weight-2b-class-models/
source_anchor: ""
source_lines: [1, 43]
section: "Liquid_LFM"
---
## Key PointsLiquid AI unifies text vision audio and decision models on one shared backbone rather…

View more

- Liquid AI unifies text vision audio and decision models on one shared backbone rather than separate architectures.
- Hardware-in-the-loop architecture search across phones laptops and cloud chips lets smaller models rival far larger rivals.
- Every LFM3 model can be fine-tuned on a single GPU making specialized deployment accessible to small teams.

Liquid AI announced LFM3 on October 27, 2026, a new generation of open-weight Liquid Foundation Models that brings text, vision, audio, omni, and decision models onto one shared backbone. The first release, detailed in a post on the Hugging Face blog, covers six models in the 2B parameter range.

## Six Models on One Shared Backbone

Every LFM3 model branches from the same LFM3-1.7B-Base. LFM3-1.7B takes text in and returns text. LFM3-VL-2B accepts text, images, and video and returns text, while LFM3-Audio-2B takes text or audio and responds with text or speech. LFM3-Omni-2B, which Liquid AI describes as its first omni model, accepts text, images, video, and audio and produces text and speech. Two decision models, d1-2B and d1-omni-2B, are specialized from the vision and omni models and return calibrated answers rather than generated text.

According to the LFM3-1.7B model card, the text model has 1.73B parameters, a 256K-token context length, a 128,000-token vocabulary, and support for 16 languages, and it ships under the LFM Open License v1.0. The base and post-trained models are open-weight and available on Hugging Face, the Liquid AI Playground, and the Liquid AI API.

Liquid AI’s release post names day-0 launch partners Ambarella, AMD, Arm, Intel via OpenVINO, NVIDIA, OpenRouter, Qualcomm via Qualcomm AI Hub and GenieX, and Vercel, and says Mercedes-Benz and Shopify partnered on pre-release testing and evaluation.

## Tri-Hybrid Architecture and Training

LFM3-1.7B-Base has 24 layers: 13 gated short convolutions, five sliding-window attention layers with a 256-token window, and six global attention layers. The convolutions use a kernel size of 7, up from 3 in LFM2. Three further changes define the backbone: head-wise gated attention, in which a sigmoid gate scales each attention head’s output and can suppress activation outliers that hurt quantization; partial RoPE, under which global attention layers rotate only the highest-frequency half of each head while sliding-window layers keep full RoPE; and a sandwich norm that normalizes each block’s input and output.

The design came from Liquid AI’s hardware-in-the-loop architecture search, expanded for LFM3 to 14 device and backend combinations, including phones (Samsung Galaxy S26 Ultra and OnePlus 15), a Raspberry Pi 5, laptops (Apple M4 Max and AMD Ryzen AI), Arm and x86 cloud CPUs, GPUs, and the Qualcomm Hexagon NPU.

LFM3-1.7B-Base was pre-trained on 22T tokens, with the final 3.5T tokens forming a mid-training stage on curated data with a larger share of agentic traces and tool calls that also extended the context window to 256K tokens. Post-training ran in four stages: supervised fine-tuning on 1.75T tokens, mixed reinforcement learning, multi-domain on-policy distillation from six specialist reinforcement-learning teachers, and agentic reinforcement learning inside real agent harnesses.

Liquid AI said the new architecture lets LFM3-1.7B outperform its predecessor LFM2.5-2.6B with 35% fewer parameters, and that per parameter it matches LFM2-1.2B on short prompts while running about 25% faster with 8K tokens in context. At 4-bit precision, the company said, the text model takes about 1 GB of memory.

## Company-Reported Performance and Specialization

Liquid AI reports that LFM3-1.7B scores 38.42 on WebArena-Verified against 3.02 for LFM2.5-2.6B, with the largest gains on agentic tasks, though the model card states the model is not recommended for agentic coding or complex multi-step reasoning because of its small size.

Liquid AI said LFM3-VL-2B is the first LFM vision model to accept video input, and reports that it raises the average across 62 vision benchmarks from 57.6 to 71.4 compared with LFM2.5-VL-3B while using 1.9B parameters against 3.1B, putting it on par with Gemma 4 12B, a model more than six times larger. A new in-house 164M-parameter vision encoder uses 3x less compute than the SigLIP2 encoder in previous LFM VL models and matches the accuracy of Qwen3.5-2B’s vision encoder with 2.2x fewer FLOPs, according to the company’s measurements.

LFM3-Audio-2B supports 18 languages, and Liquid AI reports speech quality of 3.83 on UTMOSv2 against 3.22 for LFM2.5-Audio-1.5B, a fully causal audio encoder with time to first token as low as 50 ms, and a LiquidCodec tokenizer that generates speech at about 87x real time on a single CPU thread on an Apple M4 Max. The company said the 2.2B-parameter LFM3-Omni-2B beats models up to 10.7B parameters, including Qwen2.5-Omni-7B, on the Daily-Omni, WorldSense, and AV-SpeakerBench audio-visual understanding benchmarks.

The decision models answer a list of questions in a single forward pass, each with a calibrated probability, and add typed answers that point at objects, read values from documents, estimate numbers with a range, and pick every option that applies. Liquid AI said d1-2B replaces d1-3B with a third fewer parameters and scores 75.5 against 64.0 on ImajevBench.

Every LFM3 model can be fine-tuned on a single GPU, and Liquid AI published notebooks covering supervised fine-tuning, DPO, GRPO, and continued pre-training for the text, vision, audio, and omni models. In one company example, LFM3-VL-2B was fine-tuned on about 3,000 public wind-turbine blade photos for a few hours on one A100, raising defect classification accuracy to 67% and detection F1 from 0.003 to 0.11 on held-out photos.

Liquid AI also adapted LFM3-VL-2B into three robotics models for orchestration, navigation, and manipulation. The company reports that the manipulation vision-language-action model, trained across 15 embodiments, achieved 96.2% average success across the four LIBERO suites and 45.7% on RoboCasa-GR1 tabletop tasks while generating action chunks in under 50 ms on a Jetson AGX Orin with TensorRT FP16.

On throughput, Liquid AI said LFM3-1.7B can produce 3.6 billion tokens per day on a single NVIDIA B300 at 1,024 concurrent requests. The models ship with native support for llama.cpp, MLX, and vLLM across Apple, AMD, NVIDIA, and Qualcomm hardware, with checkpoint variants including BF16, FP8, NVFP4, GGUF, MLX, and ONNX.
