---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-launches-nemotron-3-5-lightning
title: "NVIDIA launches Nemotron 3.5 Lightning"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-20"]
keywords: ["nemotron", "lightning", "model", "weights", "intelligence", "nvidia", "gpt-oss-120b", "nano"]
source: https://artificialanalysis.ai/articles/nemotron-3-5-lightning-launch
source_anchor: ""
source_lines: [1, 58]
section: "NVIDIA_Hardware"
---
# NVIDIA has just released the first Nemotron 3.5 model: Nemotron 3.5 Lightning, a highly efficient small open weights model with performance similar to gpt-oss-120b at around a quarter of the total parameters

See model page
Nemotron 3.5 Lightning is the successor to NVIDIA Nemotron 3 Nano 30B A3B, with 31.6B total and 3.6B active parameters. It retains the same hybrid Mamba-Transformer architecture and small size from Nemotron 3 Nano, but makes substantial gains in intelligence and agentic performance.

**Key takeaways:**

➤ **Major intelligence jump:** Nemotron 3.5 Lightning scores 24 on the Artificial Analysis Intelligence Index, a +9 point improvement over Nemotron 3 Nano (15). This puts it in line with OpenAI's gpt-oss-120b (24) and only just behind Nemotron 3 Super (26), a model ~4x its size

➤ **Optimized for efficiency:** Nemotron 3.5 Lightning sits behind the most intelligent small models in its size class such as Qwen3.6 35B A3B (32) and Muse Glimmer (high, 35) - but it is built for a different point on the frontier. In pre-release testing of a DeepInfra endpoint serving the final NVFP4 weights, we measured median output speeds of nearly 670 tokens per second, much faster than those models are served in the market today

➤ **Meaningful agentic gains:** the largest improvements over Nemotron 3 Nano come on agentic evaluations in GDPval-AA v2 (+334 ELO, moving past gpt-oss-120b and Nemotron 3 Super) and Terminal-Bench v2.1 (24% vs 7%). Combined with its speed and permissive OpenMDW-1.1 license, this positions Lightning as an efficient workhorse model for high-volume agentic deployments

➤ **Near-lossless NVFP4 quantization:** as with prior Nemotron releases, the model ships in NVFP4 alongside BF16 weights. We measured the NVFP4 variant at 24 on the Intelligence Index and saw minimal degradation compared to the higher-precision weights

**Key model details:**

➤ 1 million token context window, text-only reasoning model

➤ 31.6B total and 3.6B active parameters

➤ Released under the OpenMDW-1.1 license, open for commercial use without material restrictions

➤ The model weights are available now along with serverless inference from providers including DeepInfra, Fireworks, FriendliAI, CoreWeave, GMI Cloud, Nebius, and Crusoe


Nemotron 3.5 Lightning is highly performant: across the Artificial Analysis Intelligence Index, its Time per Intelligence Index Task is ~0.5 minutes based on output speeds achieved with a pre-release DeepInfra endpoint and the final NVFP4 weights.

This is substantially faster than open weights peers, well ahead of Qwen3.6 35B A3B (~3.5 min), gpt-oss-120b (~3.4 min), Gemma 4 31B (~5.8 min) and Qwen3.6 27B (~7.3 min). NVIDIA has worked with partners including CodeRabbit and Harvey to post-train Nemotron 3.5 Lightning to perform better within domains, aiming to leverage easy training to achieve strong efficiency and performance for user-specific workflows.

However, proprietary models still perform strongly on the overall time-efficiency frontier: Gemini 3.5 Flash-Lite achieves an Intelligence Index of 37 at a similar time per task, and GPT-5.6 Luna (max) scores 52 at under 2 minutes per task.


This time per task is driven by extremely high output speeds along with solid token efficiency. Nemotron 3.5 Lightning used a similar number of output tokens per task to Nemotron 3 Nano while delivering its +9 point Intelligence Index gain, and on the pre-release DeepInfra endpoint we measured output speeds of nearly 670 tokens per second.


Nemotron 3.5 Lightning's agentic capabilities are a step change for the Nemotron family's small models. Its GDPval-AA v2 Elo of 824 surpasses both Nemotron 3 Super and gpt-oss-120b, while its Terminal-Bench v2.1 score of 24% is >3x Nemotron 3 Nano's 7% and almost matches gpt-oss-120b. For a model of this size and speed, this makes Lightning an attractive model for agentic pipelines.


#### Read the latest

### Thinking Machines has released Inkling 2 Mini, an open weights looped transformer

Thinking Machines has released Inkling 2 Mini at 20 on the Artificial Analysis Intelligence Index, an open weights looped transformer on the Pareto frontier for Intelligence vs. Active Parameters

November 20, 2026

### Google continues to accelerate its release cadence with Gemini 4 Neon

Google continues to accelerate its release cadence with Gemini 4 Neon, delivering a small Intelligence gain over Gemini 4 Carbon at a ~17% lower Cost per Task

November 18, 2026

### OpenAI has the #2 model in the Intelligence Index with the release of GPT-6.2 Astra and GPT-6.2 Sol

OpenAI’s release of GPT-6.2 Astra and GPT-6.2 Sol delivers the #2 model in the Artificial Analysis Intelligence Index and most of the Cost per Task Pareto frontier

November 17, 2026
