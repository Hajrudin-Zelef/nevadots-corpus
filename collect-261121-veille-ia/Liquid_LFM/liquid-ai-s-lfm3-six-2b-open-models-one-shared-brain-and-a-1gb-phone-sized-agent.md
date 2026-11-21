---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-s-lfm3-six-2b-open-models-one-shared-brain-and-a-1gb-phone-sized-agent
title: "Liquid AI's LFM3: Six 2B Open Models, One Shared Brain — and a 1GB Phone-Sized Agent"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["liquid", "model", "text", "models", "lfm3", "shared", "agent", "vision"]
source: https://dev.to/paullovelace/liquid-ais-lfm3-six-2b-open-models-one-shared-brain-and-a-1gb-phone-sized-agent-40co
source_anchor: ""
source_lines: [1, 87]
section: "Liquid_LFM"
---
Liquid AI announced LFM3 on October 27, 2026, and the pitch is the opposite of the usual bigger-is-better launch: six open-weight models, all around 2 billion parameters, all branching from one shared backbone, built to run where the big models cannot — on a phone, a laptop, a Raspberry Pi. The text model, at 4-bit precision, takes about 1 GB of memory. That is the whole story in one number: an agent model small enough to live on the device it serves.

*Disclosure: no affiliate links in this article. All benchmark figures below are Liquid AI's own measurements from its launch announcement and model cards — they have not been independently verified yet, and I say so wherever they appear. I have not run the models hands-on; this is researched analysis, not a benchmark I performed. The licence summary is a plain-language reading, not legal advice.*

Every LFM3 model branches from the same base, LFM3-1.7B-Base. That shared backbone is the point: learn one family, and the text, vision, audio and decision variants behave consistently.

- 
**LFM3-1.7B** — text in, text out. 1.73B parameters, 256K-token context, 128,000-token vocabulary, 16 languages.
- 
**LFM3-VL-2B** — text, images and video in, text out. Liquid's first vision model to accept video input.
- 
**LFM3-Audio-2B** — text or audio in, text or speech out, across 18 languages.
- 
**LFM3-Omni-2B** — Liquid's first omni model: text, images, video and audio in, text and speech out.
- 
**d1-2B and d1-omni-2B** — decision models, specialised from the vision and omni models. Instead of generating prose, they return calibrated answers — probabilities, typed answers pointing at objects, values read from documents, number estimates with a range — in a single forward pass.

The weights are open on Hugging Face, and also available through the Liquid AI Playground and API. Checkpoint variants cover the runtimes developers actually use: BF16, FP8 and NVFP4 for vLLM and SGLang, GGUF for llama.cpp and Ollama, MLX for Apple Silicon, and ONNX for ONNX Runtime.

LFM3-1.7B-Base has 24 layers: 13 gated short convolutions, five sliding-window attention layers with a 256-token window, and six global attention layers. Liquid made three changes over the previous generation: head-wise gated attention that can suppress the activation outliers which hurt quantisation, partial RoPE on global layers, and a sandwich norm on each block's input and output.

The architecture was chosen by a hardware-in-the-loop search expanded to 14 device and backend combinations — phones (Samsung Galaxy S26 Ultra, OnePlus 15), a Raspberry Pi 5, laptops (Apple M4 Max, AMD Ryzen AI), Arm and x86 cloud CPUs, GPUs and the Qualcomm Hexagon NPU. That is why the footprint claim is credible as a design goal: the model was searched against real edge hardware, not just server GPUs.

Training scale is serious for a model this size: 22 trillion pre-training tokens, with a final 3.5T-token mid-training stage on curated data with more agentic traces and tool calls, which also stretched the context to 256K tokens. Post-training ran in four stages — supervised fine-tuning on 1.75T tokens, mixed reinforcement learning, distillation from six specialist teachers, and agentic reinforcement learning inside real agent harnesses.

Again: these are Liquid's claims, not independent results. But the honest limits are as informative as the wins.

Liquid says LFM3-1.7B outperforms its predecessor LFM2.5-2.6B with 35% fewer parameters, and runs about 25% faster than LFM2-1.2B at 8K tokens in context. On WebArena-Verified it reports 38.42 against 3.02 for LFM2.5-2.6B — a huge jump on agentic tasks, which is exactly where the model is aimed.

For vision, Liquid reports the VL model raising the average across 62 vision benchmarks from 57.6 to 71.4 against LFM2.5-VL-3B, while using 1.9B parameters instead of 3.1B — putting it, on Liquid's measurements, on par with Gemma 4 12B, a model more than six times larger. For audio, speech quality of 3.83 on UTMOSv2 against 3.22 for the previous audio model, with time to first token as low as 50 ms. For omni, Liquid says the 2.2B omni model beats models up to 10.7B, including Qwen2.5-Omni-7B, on audio-visual understanding benchmarks.

Then the limit, stated on the model card itself: **LFM3-1.7B is not recommended for agentic coding or complex multi-step reasoning because of its small size.** Use retrieval or search for knowledge-heavy tasks. That is a refreshingly straight admission, and it tells you exactly where this model belongs: local agents, tool calling, structured outputs and domain-specific fine-tuning — not as a drop-in replacement for a frontier coding model.

On throughput, Liquid says LFM3-1.7B can produce 3.6 billion tokens per day on a single NVIDIA B300 at 1,024 concurrent requests. And every model can be fine-tuned on a single GPU, with published notebooks for supervised fine-tuning, DPO, GRPO and continued pre-training. In one company example, the VL model was fine-tuned on about 3,000 public wind-turbine blade photos for a few hours on one A100, raising defect classification accuracy to 67%.

LFM3 ships under the LFM Open License v1.0 — not Apache 2.0, and the difference matters. Anyone may download, use, modify and share it, including fine-tuned versions. But commercial use is free only below a threshold: companies with annual revenue of $10 million or more need a separate commercial agreement with Liquid AI. Qualified non-profits are exempt from the threshold for non-commercial or research use.

For a solo developer, a startup or a small agency, this is effectively free. For a company at or above the threshold — or one planning to grow past it — it is a model you can test but not ship without talking to Liquid first. Factor that in now, not at your Series B. This is a summary, not legal advice: the licence file sits beside the model on Hugging Face and is worth five minutes before you deploy anything commercial.

Day-0 launch partners signal where Liquid expects this to run: Ambarella, AMD, Arm, Intel via OpenVINO, NVIDIA, OpenRouter, Qualcomm via Qualcomm AI Hub and GenieX, and Vercel — with Mercedes-Benz and Shopify named as pre-release testing partners. That is an edge and device story, not a data-centre story.

**Use LFM3 for:** local agents on phones and laptops, tool calling, structured outputs, document and image understanding on-device, voice agents where latency matters, and single-GPU domain fine-tuning.

**Use the decision models for:** classification, extraction and routing where you need a calibrated probability, not prose — moderation, triage, form-field extraction, multi-label picks.

**Do not use LFM3 for:** agentic coding, complex multi-step reasoning, or knowledge-heavy question answering without retrieval. The model card says so itself.

**Check before shipping commercially:** the $10M revenue threshold in the LFM Open License v1.0.

1. 
**Start on the Playground or the GGUF build.** The fastest honest test is the model running locally on your own machine — llama.cpp or Ollama for the text model, MLX on Apple Silicon.
2. 
**Test it on your agent harness, not a benchmark.** LFM3 is post-trained for harnesses such as Hermes Agent, Pi and OpenClaw. Your tool-calling workload is the benchmark that matters.
3. 
**Fine-tune on one GPU before you dismiss the size.** The published notebooks cover SFT, DPO, GRPO and continued pre-training — a few thousand domain examples can move a small model further than a bigger general one.
4. 
**Use the decision models where you would otherwise parse prose.** A calibrated probability plus a typed answer is easier to trust in production than a paragraph you have to regex.
5. 
**Wait for independent benchmarks before replacing a production model.** One run on your own workload beats ten launch charts.

LFM3 is not exciting in the way a flagship launch is exciting, and that is exactly why it matters. The agent era does not live only on the smartest model in the cloud. It also lives on the model that runs privately, instantly, on the device in your hand — no round trip, no API bill, no data leaving the room.

Liquid just shipped six of those models, open, on one backbone, at a size a phone can hold. If independent testing backs even half of the vendor claims, LFM3 becomes the default to evaluate for local-agent work — not because a 1.7B model beats a frontier model, but because it is the smallest one good enough for the job, and it runs where the frontier model cannot go.

**What is Liquid AI LFM3?**

A family of six open-weight models in the 2B parameter range announced October 27, 2026, all branching from the same LFM3-1.7B-Base backbone: text, vision, audio, omni, and two decision models.

**Can LFM3 run on a phone?**

Liquid AI says the 1.73B text model takes about 1 GB of memory at 4-bit precision, and its architecture search covered phones, a Raspberry Pi 5, laptops and NPUs — but that is a footprint claim, not a promise that every phone runs every model well.

**Is LFM3 free for commercial use?**

Under the LFM Open License v1.0, commercial use is free below the $10 million annual revenue threshold; companies at or above that threshold need a separate commercial agreement with Liquid AI.

**Where do I get LFM3?**

Open weights on Hugging Face, plus the Liquid AI Playground and API, with GGUF, MLX, ONNX, BF16, FP8 and NVFP4 checkpoint variants for llama.cpp, MLX, vLLM, SGLang and ONNX Runtime.

**Is LFM3 good for coding?**

Not according to its own model card: LFM3-1.7B is not recommended for agentic coding or complex multi-step reasoning because of its small size. Its strength is local agents, tool calling, structured outputs and domain fine-tuning.

**Sources:** Liquid AI's LFM3 release post and the LFM3-1.7B model card on Hugging Face, as reported by Unite.AI and Liquid's own documentation. Benchmark figures are Liquid AI's own and have not been independently verified.

## Top comments (0)
