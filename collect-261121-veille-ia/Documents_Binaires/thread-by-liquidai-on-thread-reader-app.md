---
id: collect-261121-veille-ia/documents-binaires/thread-by-liquidai-on-thread-reader-app
title: "Thread by @liquidai on Thread Reader App"
domain: documents-binaires
role: reference
task: article
actors: []
dates: []
keywords: ["architecture", "model", "lfm3-1", "models", "thread", "text", "lfm3", "audio"]
source: https://threadreaderapp.com/scrolly/2115128329003303107
source_anchor: ""
source_lines: [1, 237]
section: "Documents_Binaires"
---
Introducing 𝙻𝙵𝙼𝟹: our new generation of Liquid Foundation Models. With a new tri-hybrid architecture and new multimodal encoders, the LFM3 family is the ideal model suite for specialized intelligence.


> 6 model instances, across text, audio, vision, and omni modalities, generative and decision models.

> 𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱: text, 256k context window.

> 𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱: vision-language model with video-input support for spatial understanding

> 𝙻𝙵𝙼𝟹-𝙰𝚞𝚍𝚒𝚘-𝟸𝙱: tool calls, voice chat, reasoning in 18 languages.

> 𝙻𝙵𝙼𝟹-𝙾𝚖𝚗𝚒-𝟸𝙱: an omni model combines vision, audio, and language.

> 𝚍𝟷-𝟸𝙱 and 𝚍𝟷-𝙾𝚖𝚗𝚒-𝟸𝙱: decision models with typed outputs and calibrated probabilities in a single pass. Omni adds audio input.

> fine-tune on 1 GPU.

> a new tri-hybrid architecture

> architecture optimized for 14 processor classes across GPUs, CPUs, and NPUs.

> 𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 is a beast. It performs competitively with Gemma 4 12B (6x larger), and Gemini 3.5 Flash Lite, and GPT-5.6 Luna, with 1.9B parameters.

> 𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱 is #1 on the Pareto frontier of device intelligence 𝚙𝚒𝚙𝚎𝚝𝚝𝚎.𝚕𝚒𝚚𝚞𝚒𝚍.𝚊𝚒


Every modality for specialized intelligence. Pick the one that fits your use case, customize it, and run it on-device.


1/  🧵

LFM3 is our most capable generation of foundation models yet, built for specialization and on-device deployment. Here’s what’s new:


> More capability per parameter: a new tri-hybrid architecture combines gated short convolutions, sliding-window attention, and global attention. Together with an updated training recipe, it lets 𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱 outperform LFM2.5-2.6B with 35% fewer parameters.


> Every modality, one backbone: six 2B-class models for text, vision, audio, omni, and decision tasks, all built on 𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱-𝙱𝚊𝚜𝚎.


> Built for the edge: The text model’s weights fit in about 1GB at 4-bit precision, and it supports a 256K-token context window for longer agent workflows.


> Made to be specialized: Each model can be fine-tuned on a single GPU. Adapt it to your data, terminology, and tasks. Then deploy on devices, private infrastructure, or in the cloud.


2/

We used our proprietary automated hardware-in-the-loop architecture search to design LFM3’s architecture for agentic workloads, targeting more capability per parameter and fast inference over long contexts.


For LFM2, the search focused on CPUs on phones and laptops. For LFM3, we expanded to 14 device and backend combinations: phones (Galaxy S26 Ultra and OnePlus 15), laptops (Apple M4 Max and @AMD Ryzen AI), a Raspberry Pi 5, and @Arm and x86 cloud CPUs, plus GPUs and the @Qualcomm Hexagon NPU.


The search evaluates model quality alongside measured inference speed and memory use on target hardware. This makes deployment constraints part of architecture selection, optimizing for stronger capabilities within the latency and memory budgets of the devices running your agents.


3/

𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱 powers local text agents with a 256K-token context window. Fine-tune it on a single GPU for your domain, harness and tools, then run it on your own hardware without per-token API fees.


Post-trained in agent harnesses such as Hermes Agent, Pi, and OpenClaw, it can triage your inbox, manage your calendar, research the web, send you morning briefings on WhatsApp, and find the right tool among thousands of options.


In our evaluations, it outperforms Qwen3.5-9B on WebArena-Verified and AutomationBench, which test multi-step tasks in browsers and business applications.


At 128K tokens, its KV cache occupies 1.6 GB, compared with 4.3 GB for Qwen3.5-4B. That leaves more memory for your application as task histories grow.

It also scales to server workloads. In our serving tests, p95 request latency remains stable under heavy traffic, supporting latency-sensitive applications such as online fraud detection.


To demonstrate that scale, we ran more than 300,000 LFM3-1.7B agents in parallel to investigate the Enron email archive.


> 𝙻𝙵𝙼𝟹-𝟷.𝟽𝙱 model card: huggingface.co/LiquidAI/LFM3-…


4/

𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 brings video understanding, step-by-step reasoning, and spatial understanding to local assistants. Ask questions about a recorded lecture, a walkthrough filmed on your phone or a clip from a camera feed. It reasons through visual problems involving charts, mathematics and multiple steps. It can also locate objects and interpret their spatial relationships, providing a foundation for robotics applications.


> Across 62 vision benchmarks, it averages 71.4, comparable with Gemma 4 12B’s 71.4, with just 1.9B parameters.

> 𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 performs competitively with Gemma 4 12B (6x larger), and Gemini 3.5 Flash Lite, and GPT-5.6 Luna, with 1.9B parameters.

> Our new in-house vision encoder uses about one-third of the compute of the SigLIP2 encoder.


> 𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 model card: huggingface.co/LiquidAI/LFM3-…


5/

𝙻𝙵𝙼𝟹-𝙰𝚞𝚍𝚒𝚘-𝟸𝙱 connects voice conversations to actions on your own hardware. It accepts text or audio and responds with text, speech or both, with support for 18 languages.


It can interpret speech, environmental sounds and events in recordings, answer spoken questions, and turn spoken requests into tool calls that control your applications and devices. Responses can combine tool calls with text or speech.


Its new causal audio encoder processes microphone input while you speak, reducing the wait after you finish. For text-only responses, the model can reason before answering.


> On our internal spoken tool-calling evaluation, it scores 58.5, compared with 44.3 for Qwen2.5-Omni-3B.

> LiquidCodec, our new audio tokenizer, converts generated tokens into speech at about 87× real time on a single Apple M4 Max CPU thread.


Voice interaction, audio understanding and tool use, running locally on laptops and edge devices.


> 𝙻𝙵𝙼𝟹-𝙰𝚞𝚍𝚒𝚘-𝟸𝙱 model card: huggingface.co/LiquidAI/LFM3-…


6/

𝙻𝙵𝙼𝟹-𝙾𝚖𝚗𝚒-𝟸𝙱 is our first omni model, built for cross-modal interactions that require understanding visual and auditory information together. It combines vision, audio and language in a single model that responds in text, speech or both.


Ask a spoken question about an image, or provide a video with its soundtrack and ask something that requires understanding both. The model processes visual and auditory information jointly.


It also combines visual context with spoken instructions to generate structured tool calls, helping assistants act on what users show and tell them.


> In our evaluations, it scores 70.1 on Daily-Omni, which tests video and audio understanding, compared with 61.3 for Qwen2.5-Omni-7B.


At 2.2B parameters, it brings multimodal agents on-device that use visual and auditory context to guide users, respond to voice commands and act through tools. Developers can specialize this foundation model for personalized education, everyday assistance, in-car experiences, digital interfaces, robotics, and more.


> 𝙻𝙵𝙼𝟹-𝙾𝚖𝚗𝚒-𝟸𝙱 model card: huggingface.co/LiquidAI/LFM3-…


7/

𝚍𝟷-𝟸𝙱 and 𝚍𝟷-𝙾𝚖𝚗𝚒-𝟸𝙱 are decision models that turn documents, images and audio into structured answers your applications can act on.


𝚍𝟷-𝟸𝙱 accepts text and images. 𝚍𝟷-𝙾𝚖𝚗𝚒-𝟸𝙱 adds audio. Both answer multiple questions about the same input in a single forward pass, with calibrated probabilities.


New typed outputs extend their capabilities beyond yes/no answers, choices and scores:


> Locate objects with bounding boxes, or find and count every match.

> Extract text, numbers, and dates with their source spans, returning “not found” when a field is absent.

> Estimate numerical values with a likely range.

> Select every applicable option, each with its own probability.


Route support requests, extract invoice fields or inspect camera frames locally, without a round trip to the cloud.


On our index covering 117 public and private benchmarks, d1-2B scores 63.2 versus 58.8 for d1-3B, with a third fewer parameters.


> 𝚍𝟷-𝟸𝙱 model card: huggingface.co/LiquidAI/d1-2B 

> 𝚍𝟷-𝙾𝚖𝚗𝚒-𝟸𝙱 model card: huggingface.co/LiquidAI/d1-Om…


8/

Frontier models bring broad capabilities, but specialized tasks often require deeper knowledge of your domain. LFM3’s 2B-class models are built to be fine-tuned on your own data and deployed where your application runs.


For wind turbine inspection, we fine-tuned 𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 on approximately 3,000 public WTBs2025 photos using SFT and GRPO. Training took a few hours on one A100.


On approximately 1,400 held-out photos:

> Defect classification accuracy reached 67%, compared with under 10% for both the untuned model and GPT-6 Luna.

> Detection F1 improved from 0.003 to 0.11, compared with 0.01 for GPT-6 Luna.

The fine-tuned model can also answer questions about the images and write inspection findings describing the defect, supporting evidence and location.


9/

𝙻𝙵𝙼𝟹-𝚅𝙻-𝟸𝙱 can also serve as a foundation for robotics. We explore three adaptations spanning the robotics hierarchy, from tool-based task planning to navigation and manipulation.


Tool orchestration: ground objects by pointing, measure them with depth tools, call motion planners, and check outcomes in new camera images. Pair it with a fine-tuned d1-2B skill monitor to detect failures and guide recovery.


Navigation: translate route instructions and a sequence of image observations into visually grounded waypoint trajectories. The model achieves 53% success on R2R and 43% on RxR in unseen scenes under strict continuous-control evaluation with wall sliding disabled.


Manipulation: turn images, language, and robot state into multi-step action chunks in one forward pass. Jointly trained across 15 embodiments, the resulting VLA achieves 96.2% average success across the four LIBERO suites and 45.7% on RoboCasa-GR1 tabletop tasks.


The manipulation VLA generates action chunks in under 50 ms on an @nvidia Jetson AGX Orin using TensorRT in FP16, enabling real-time action generation on embedded hardware.


10/

Thank you to our day-0 launch partners for making LFM3 available across hardware and developer platforms:


@AMD, @Arm, @NVIDIA, @OpenRouter, @Qualcomm (AI Hub and GenieX), and @vercel, @Ambarella_Inc, and @Intel (OpenVINO).


To our partners and customers, @MacPaw, @MercedesBenz, and @Shopify for pre-release testing and evaluation of LFM3.


And to the teams helping developers run, customize and build with LFM3:


@huggingface, llama.cpp (@ggml_org), @radixark with SGLang (@sgl_project), @lmstudio, MLX, @ollama, @dottxtai, @unslothai, @vllm_project, @zml_ai, @atomic_chat_hq, and @cactuscompute.

 

11/

Start building with 𝙻𝙵𝙼𝟹


> Blog: liquid.ai/blog/lfm3

> Docs: docs.liquid.ai

> Open weights: huggingface.co/collections/Li…

> Playground: playground.liquid.ai

> API: console.liquid.ai

> Pipette: pipette.liquid.ai 

> Get in touch for custom fine-tuning: liquid.ai/connect
