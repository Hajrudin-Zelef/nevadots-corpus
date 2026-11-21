---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-120b-a12b-nvidia-ngc-2
title: "Nemotron-3.5-Super-VL-120B-A12B | NVIDIA NGC"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "bf16", "nvfp4", "weights", "super-vl-120b-a12b", "decoding", "available"]
source: https://catalog.ngc.nvidia.com/orgs/nim/nvidia/models/nemotron-3.5-super-vl-120b-a12b/-?_lr=1
source_anchor: ""
source_lines: [1, 724]
section: "NVIDIA_Hardware"
---
NVIDIA Nemotron 3.5 Super VL 120B-A12B vision-language model weights (BF16, NVFP4) used by the NIM.

# NVIDIA-Nemotron-3.5-Super-VL-120B-A12B — NIM model artifacts

## Description

**NVIDIA-Nemotron-3.5-Super-VL-120B-A12B** is a vision-language model (VLM) trained by NVIDIA. This repository holds the model artifacts consumed by the **NVIDIA Nemotron 3.5 Super VL NIM (2.0.13)**: the BF16 weights and the NVFP4 weights. The NIM selects the version that matches the GPU profile it runs.

The model combines interleaved Mamba-2, Mixture-of-Experts (MoE), and attention layers with a C-RADIO vision encoder and a vision-language connector. It has **120B total parameters** and **12B active parameters** and supports context lengths up to **1M tokens**. Multi-Token Prediction (MTP) enables native speculative decoding.

This model is available for commercial and non-commercial use under the OpenMDW-1.1 license.

For more details on how to deploy and use the model — see the Quick Start Guide below!

## Versions in This Repository

| Version | Model | Files | Size | Used by | 
|---|---|---|---|---|
| `bf16-ga-20261003` | `NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16` | 81 | 231.84 GB | BF16 NIM profiles | 
| `nvfp4-ga-20261003` | `NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4` | 107 | 69.91 GB | NVFP4 NIM profiles | 

Each version ships a `checksums.blake3` file. Other versions in this repository are earlier pre-release builds and are not used by the 2.0.13 NIM.

## Model Summary

| **Total Parameters** | 120B (12B active) | 
| **Architecture** | Mamba-2–Transformer hybrid Mixture of Experts with a C-RADIO vision encoder | 
| **Precision** | BF16 ( `bf16-ga-20261003` ), NVFP4 (`nvfp4-ga-20261003` ) | 
| **Inputs / Outputs** | Text, images, and video / text | 
| **Context Length** | Up to 1M tokens | 
| **Minimum GPU** | NVFP4: 1× H200 / 1× B200 / 1× GB200 / 1× B300 / 1× GB300, DGX Station, DGX Spark. BF16: 2× B200 / 2× GB200 / 2× B300 / 2× GB300 | 
| **Supported Hardware** | NVIDIA Blackwell (B200, GB200, B300, GB300, RTX Pro 6000 Blackwell Server Edition, DGX Station, DGX Spark); NVIDIA Hopper (H100, H200); NVIDIA Ampere (A100-80GB) | 
| **Supported Languages** | English, French, German, Italian, Japanese, Spanish, and Chinese | 
| **Speculative Decoding** | Native Multi-Token Prediction (MTP) — Read more below | 
| **Reasoning Mode** | Configurable through the chat template ( `enable_thinking=True/False` ) | 
| **Recommended Sampling** | Text: temperature 1.0, top_p 0.95; vision: temperature 0.6, top_p 0.95 | 
| **Best For** | Efficient deployment of text and vision-language agents | 
| **License** | OpenMDW-1.1 | 
| **Release Date** | 10/15/2026 (NIM 2.0.13) | 

## Hardware Matrix

Number of GPUs per NIM profile for each combination of precision and hardware.

| Hardware | BF16 | NVFP4 | 
|---|---|---|
| **H100 SXM 80GB** | 8 | 2 | 
| **H200 SXM** | 4 | 1 | 
| **B200** | 2 | 1 | 
| **GB200** | 2 | 1 | 
| **B300** | 2 | 1 | 
| **GB300** | 2 | 1 | 
| **A100 SXM4 80GB** | Not Available | 2 | 
| **RTX Pro 6000 Blackwell Server Edition** | Not Available | 2 | 
| **DGX Station (GB300)** | Not Available | 1 | 
| **DGX Spark (GB10)** | Not Available | 1 | 

## Speculative Decoding

| Profile | Method | Artifact | 
|---|---|---|
| All profiles | Built-in **MTP** head | included in the BF16 / NVFP4 weights | 

Speculative decoding is lossless: verified tokens are identical to non-speculative decoding.

## Model Overview

**Model Developer:** NVIDIA Corporation

### What is Nemotron?

NVIDIA Nemotron™ is a family of open models with open weights, training data, and recipes, delivering leading efficiency and accuracy for building specialized AI agents.

## License/Terms of Use

**Governing Download Terms:** Use of this model is governed by the OpenMDW License Agreement, version 1.1.

## Benchmarks

### Agentic Coding Benchmarks

The following results evaluate the post-trained Nemotron 3.5 Super model.

| Benchmark | Agent Harness | Score (%) | 
|---|---|---|
| SWE-bench Pro | Claude Code | 49.45 | 
| Terminal-Bench 2.1 | Codex | 61.80 | 

These are the highest scores across six evaluated agent harnesses, using their standard configurations and unmodified system prompts. For SWE-bench Pro, Git history is removed and the user instruction prohibits seeking online resources. Results depend on the harness and its configuration.

### Deployment Geography: Global

### Use Case

This model is intended for developers deploying vision-language agents for coding, search, tool-assisted workflows, document question answering, and image and video understanding, on NVIDIA data center and DGX Spark systems.

### Release Date

Hugging Face 10/15/2026 via Hugging Face BF16 and Hugging Face NVFP4 

## Reference(s)

## Model Architecture

- **Architecture Type:** Mamba-2–Transformer hybrid Mixture of Experts (MoE) with Multi-Token Prediction (MTP)
- **Network Architecture:** Nemotron Hybrid MoE with 512 routed experts per MoE layer, a C-RADIO vision encoder, and a vision-language connector
- **Number of Model Parameters:** 120B total / 12B active

## Model Design

The model combines sparse expert computation, attention, and Mamba-2 layers to support long-context processing. The vision encoder and connector turn visual inputs into representations processed by the language backbone. A shared MTP head predicts future tokens recursively for speculative decoding.

The model supports up to 1M tokens of context. The prompt, visual inputs, conversation history, tool results, and generated response share this context window. Individual deployment recipes may use a smaller context window to accommodate the available GPU memory and concurrency.

## Training Methodology

**Stage 1: Foundation and Multi-Token Prediction**

The language backbone is initialized from Nemotron 3 Super Base. A shared MTP head is trained for 100B tokens while the backbone remains frozen, followed by 1T tokens of distillation from Nemotron 3 Ultra Base.

**Stage 2: Continued Pre-Training and Mid-Training**

The model undergoes 100B tokens of Indic continued pre-training, a 35B-token extension to 1M context, 67B tokens of software-engineering mid-training at 256K context, and a final 9B-token long-context recovery stage. Software-engineering data includes repositories, issue–pull-request pairs, commit diffs, build and CI logs, and agent trajectories.

**Stage 3: Vision-Language Training**

A C-RADIO vision encoder and connector extend the language backbone to visual inputs, including images, document pages, and video frames.

**Stage 4: Supervised Fine-Tuning**

A two-stage supervised fine-tuning pipeline jointly trains the language and vision components on vision, reasoning, code, tools, chat, safety, and long-context data. Training uses packed sequences of up to 524,288 tokens.

**Stage 5: Reinforcement Learning and Distillation**

Reinforcement learning with verifiable rewards, reinforcement learning from human feedback, and specialized teacher training improve reasoning, agentic, chat, and vision capabilities. Multi-teacher on-policy distillation consolidates these capabilities into a single model.

**Stage 6: MTP Boosting**

The MTP head is further trained against the frozen post-trained backbone to improve speculative-decoding acceptance.

## Input

- **Input Type(s):** Text, images, and video
- **Input Format(s):** Text strings, images, and sampled video frames
  - Video: mp4, up to 2 minutes.
  - Image: Red, Green, Blue (RGB) (jpeg, png)
- **Input Parameters:** One-Dimensional (1D): Sequences
- **Other Properties Related to Input:** Maximum context length up to 1M tokens. Documents can be supplied as page images with accompanying text.

## Output

- **Output Type(s):** Text
- **Output Format:** String
- **Output Parameters:** One-dimensional sequences
- **Other Properties Related to Output:** Maximum context length up to 1M tokens.

Our AI models are designed and/or optimized to run on NVIDIA GPU-accelerated systems. By leveraging NVIDIA's hardware (e.g. GPU cores) and software frameworks (e.g., CUDA libraries), the model achieves faster training and inference times compared to CPU-only solutions.

## Software Integration

- **Runtime Engine(s):** vLLM (NVIDIA NIM)
- **Supported Hardware Microarchitecture Compatibility:** NVIDIA Ampere - A100-80GB; NVIDIA Blackwell - B200, GB200, B300, GB300, RTX Pro 6000 Blackwell Server Edition, DGX Station, DGX Spark; NVIDIA Hopper - H100, H200
- **Preferred/Supported Operating System(s):** Linux

The integration of foundation and fine-tuned models into AI systems requires additional testing using use-case-specific data to ensure safe and effective deployment. Following the V-model methodology, iterative testing and validation at both unit and system levels are essential to mitigate risks, meet technical and functional requirements, and ensure compliance with safety and ethical standards before deployment.

## Model Version(s)

- `bf16-ga-20261003` (10/03/2026)
- `nvfp4-ga-20261003` (10/03/2026)

## **Quick Start Guide**

These artifacts are downloaded and served by the NVIDIA Nemotron 3.5 Super VL NIM; you do not need to download them manually. Set `NIM_IMAGE` to the NIM container image for the release.

```
export NGC_API_KEY=<your NGC API key>
export NIM_IMAGE=<NIM container image>
export LOCAL_NIM_CACHE=~/.cache/nim
mkdir -p "$LOCAL_NIM_CACHE"
```
**1. List the profiles and pick the one for your GPU.** Pin it with `NIM_MODEL_PROFILE`; automatic selection is not reliable across GPU SKUs.

```
docker run --rm --gpus all -e NGC_API_KEY $NIM_IMAGE list-model-profiles
```
**2. Start the NIM.**

```
docker run -it --rm --gpus all --shm-size=16GB \
  -e NGC_API_KEY \
  -e NIM_MODEL_PROFILE=<profile id from step 1> \
  -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,video \
  -v "$LOCAL_NIM_CACHE:/opt/nim/.cache" \
  -p 8000:8000 \
  $NIM_IMAGE
```
`NVIDIA_DRIVER_CAPABILITIES` must include `video` so the NIM can decode video on the GPU (NVDEC).

**3. Check readiness.**

```
curl -s http://localhost:8000/v1/health/ready
curl -s http://localhost:8000/v1/models
```
### **API Client**

The examples below use the OpenAI-compatible client against the NIM endpoint. Recommended sampling settings are **temperature 1.0, top_p 0.95** for text and **temperature 0.6, top_p 0.95** for image and video.

Copy the identifier returned by `GET /v1/models` into `MODEL` below.

```
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="EMPTY")
MODEL = "model"  # replace with the identifier returned by GET /v1/models
```
NVIDIA Nemotron 3.5 Super VL exposes reasoning control through chat-template kwargs: thinking enabled (the default), and thinking disabled for direct answers. With the OpenAI SDK these are passed via `extra_body`, not as top-level arguments.

## **Text, image, and video examples: Click to expand!**

**Text**

```
response = client.chat.completions.create(
    model=MODEL,
    messages=[{"role": "user", "content": "Hello, what can you do?"}],
    max_tokens=16000,
    temperature=1.0,
    top_p=0.95,
)
print(response.choices[0].message.content)
```
Expected output:

```
Hello! I am an AI assistant designed to help you with a wide variety of tasks. Here are some of the things I can do:
*   **Answer Questions:** I can provide information on a vast range of topics, from science and history to current events and general knowledge.
*   **Writing & Editing:** I can help write emails, essays, poems, scripts, or articles. I can also proofread and improve your existing text.
*   **Coding & Tech Support:** I can help write code in various programming languages, debug errors, and explain technical concepts.
*   **Summarization:** I can condense long articles, documents, or videos into key points.
*   **Translation:** I can translate text between many different languages.
*   **Brainstorming:** I can help generate ideas for projects, business names, gifts, or creative endeavors.
*   **Math & Logic:** I can solve mathematical problems and work through logic puzzles.
Is there something specific you would like help with today?
```
Reasoning is on by default. The NIM returns the chain of thought in the `reasoning` field of the message, and `content` holds the final answer. Add `extra_body={"chat_template_kwargs": {"enable_thinking": False}}` for a direct answer.

**Image**

```
import base64
def image_to_data_url(path: str) -> str:
    with open(path, "rb") as f:
        b64 = base64.b64encode(f.read()).decode("utf-8")
    return f"data:image/jpeg;base64,{b64}"
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "image_url", "image_url": {"url": image_to_data_url("your_image.jpg")}},
                {"type": "text", "text": "Describe this image."},
            ],
        }
    ],
    max_tokens=16000,
    temperature=0.6,
    top_p=0.95,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
print(response.choices[0].message.content)
```
Expected output:

```
The image shows a capybara in a zoo enclosure. The capybara is eating green leafy vegetables on the ground. Its enclosure has a dirt floor and a large hollowed-out log behind it. There's a concrete wall visible, with trees and bushes beyond it. The capybara is facing right, with its head down as it eats. The scene is well-lit, suggesting it's daytime. The image captures a natural moment of the capybara's daily life in its zoo habitat.
```
**Video**

```
import base64
def video_to_data_url(path: str) -> str:
    with open(path, "rb") as f:
        b64 = base64.b64encode(f.read()).decode("utf-8")
    return f"data:video/mp4;base64,{b64}"
response = client.chat.completions.create(
    model=MODEL,
    messages=[
        {
            "role": "user",
            "content": [
                {"type": "video_url", "video_url": {"url": video_to_data_url("your_video.mp4")}},
                {"type": "text", "text": "Describe what happens in this video."},
            ],
        }
    ],
    max_tokens=16000,
    temperature=0.6,
    top_p=0.95,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}},
)
print(response.choices[0].message.content)
```
Expected output:

```
The video shows a rotating view of Earth from space, highlighting the illuminated areas of the planet against the dark backdrop of space. The continents and oceans are visible, with city lights indicating populated regions. The rotation provides a dynamic perspective of the Earth's surface, showcasing its curvature and the distribution of light across different landmasses.
```
For video use cases, try both enabling and disabling thinking for best results. A public `https://` video URL also works in place of the base64 data URL.

**PDF input:** The API accepts images, not raw PDF files. Render each page to PNG (e.g. with `pymupdf`) and send it as a base64 `image_url` as in the Image example above.

#### **Recommended Media Inputs**

**Video**

Use MP4 files of **1–2 minutes**. Every NIM profile samples at **2 FPS, up to 256 frames**, with Efficient Video Sampling (pruning rate 0.5), which covers roughly two minutes of footage. Longer clips are truncated to 256 frames.

**Images**

Tested input configurations:

| Input type | Resolution | Images per request | 
|---|---|---|
| Simple image | 512×512 | 1 | 
| Document image | 1536×1024 | 1 | 
| Multi-document image | 1536×1024 | 32 | 

Each page image counts toward the prompt side of the context length.

#### **Notes**

1. **Video container and length:** MP4, 1–2 minutes. Other containers are untested.
2. **Video decoding needs NVDEC access:** start the container with`NVIDIA_DRIVER_CAPABILITIES=compute,utility,video` .
3. **`max_tokens` vs context length:**`max_tokens` caps only the completion. Prompt plus completion must fit inside the profile's context length, and video prefill consumes a large share of it.

## Training, Testing, and Evaluation Datasets

### Training

**Data Modality:** Text, images, and video

**Foundation Training Data Size:** More than 20 trillion tokens
**Image Training Data Size:** 1 Million to 1 Billion Images
**Text Training Data Size:** More than 10 Trillion Tokens
**Video Training Data Size:** 10,000 to 1 Million Hours
**Foundation Data Collection Period:** 2013 to December 2025

**Data Collection Method:** Hybrid: automated, manually-collected, and synthetic

**Labeling Method:** Hybrid: automated, manually-labeled, and synthetic

The foundation corpus contains curated and synthetic text covering English, 19 other spoken languages, and 43 programming languages. Sources include webpages, dialogue, articles, code, and other written materials across math, science, legal, finance, and general knowledge domains.

The post-training corpus includes reasoning, coding, tool use, instruction following, structured outputs, chat, safety, and long-context tasks, together with image and video understanding. Synthetic trajectories are filtered for structural correctness, repetition, and task validity.

For more information about the datasets used to train this model, please see the Nemotron 3.5 Super VL EU Training Content Summary.

## Detailed dataset information

#### **Base Pre-Training Corpus (Nemotron 3 Foundation)**

The foundation of the model is trained on the Nemotron 3 corpus, comprising the following datasets from the Nemotron Pretraining Datasets collection:

| Dataset Collection | Token Counts | Description | 
|---|---|---|
| **Nemotron-CC-v2** &**v2.1** | 9.1T | A massive collection of English web data filtered from Common Crawl, including 2.5T+ tokens of new organic, translated, and synthetically rephrased content. | 
| **Nemotron-CC-Code-v1** | 427.9B | High-quality code tokens extracted from Common Crawl using the Lynx + LLM pipeline to preserve structure and equations. | 
| **Nemotron-Pretraining-Code-v1** &**v2** &**v3** | 1.7T | Curated GitHub code references with multi-stage filtering, deduplication, and large-scale synthetic code data. | 
| **Nemotron-CC-Math-v1** | 133.3B | High-quality math pre-training dataset preserving LaTeX formatting and mathematical structures. | 
| **Nemotron-Pretraining-Specialized-v1** &**v1.1** &**v1.2** &**Nemotron-Pretraining-SFT-v1** | 660.0B | Synthetic datasets targeting specialized domains such as STEM reasoning and scientific coding. | 
| **Nemotron-Pretraining-Legal-v1** | 4.3B | Synthetic datasets targeting the legal domain. | 

### Public Datasets

### **Crawled and Scraped from Online Sources by NVIDIA**

The English Common Crawl data was downloaded from the Common Crawl Foundation (see their FAQ for details on their crawling) and includes the snapshots CC-MAIN-2013-20 through CC-MAIN-2025-13. The data was subsequently deduplicated and filtered in various ways used in the Nemotron-CC data pipeline. Additionally, we extracted data for fifteen languages from the following three Common Crawl snapshots: CC-MAIN-2024-51, CC-MAIN-2025-08, CC-MAIN-2025-18. The fifteen languages included were Arabic, Chinese, Danish, Dutch, French, German, Italian, Japanese, Korean, Polish, Portuguese, Russian, Spanish, Swedish, and Thai. As we did not have reliable multilingual model-based quality classifiers available, we applied just heuristic filtering instead—similar to what we did for lower quality English data in the Nemotron-CC pipeline, but selectively removing some filters for some languages that did not work well. Deduplication was done in the same way as for Nemotron-CC.

The GitHub Crawl was collected using the GitHub REST API and the Amazon S3 API. Each crawl was operated in accordance with the rate limits set by its respective source, either GitHub or S3. We collect raw source code and subsequently remove any having a license which does not exist in our permissive-license set.

| Dataset | Modality | Dataset Size | Collection Period | Collecting Organisation | 
|---|---|---|---|---|
| English Common Crawl | Text | 3.36T | 4/8/2025 | NVIDIA Advanced Deep Learning Research | 
| English Common Crawl 1.1 | Text | Not disclosed | 10/2/2025 | NVIDIA Advanced Deep Learning Research | 
| Multilingual Common Crawl | Text | 812.7B | 5/1/2025 | NVIDIA Advanced Deep Learning Research | 
| GitHub Crawl | Text | 747.4B | 4/29/2025 | NVIDIA Advanced Deep Learning Research | 
| GitHub Crawl 1.1 | Text | 172.7B | 9/30/2025 | NVIDIA Advanced Deep Learning Research | 
| Sangraha | Text | 109,263,115 indexed source sequences: 77,231,480 verified and 32,031,635 unverified | Undisclosed | NVIDIA Advanced Deep Learning Research | 
| LITA Chunked Video Labels, Custom No-Thinking Dense Captions | Text (video labels) | 151,349 records | Undisclosed | NVIDIA Advanced Deep Learning Research | 
| LITA Chunked Video Labels, Cleaned v2 | Text (video labels) | 150,434 records | Undisclosed | NVIDIA Advanced Deep Learning Research | 

## Private Non-publicly Accessible Datasets of Third Parties

| Dataset | Model(s) used | 
|---|---|
| Global Regulation | Unknown | 
| TAUS Translation Memory | Unknown | 
| Scale HLE | Unknown | 
| HackerRank Coding | Unknown | 
| RL data for Search | Gemini 3; GPT-5 | 
| Mercor SWE-AgentsV1 | Undisclosed | 
| KeenSight Batch 1, Relabeled Subset | Qwen3.5-122B-A10B | 
| KeenSight Batch 1, Remaining Subset | Qwen3.5-122B-A10B | 
| Camio Multilingual OCR and Document Layout | Undisclosed | 
| Chinese Visual Question Answering, Relabeled Subset | Qwen3.5-122B-A10B | 
| Camio Chinese OCR, Cleaned Variant | Qwen3.5-122B-A10B | 
| Camio English OCR, Cleaned Variant | Qwen3.5-122B-A10B | 
| KeenSight Batch 2, Remaining Subset | Qwen3.5-122B-A10B | 
| KeenSight Batch 1, Safety-Filtered Trace Variant | Qwen3.5-122B-A10B | 
| Chinese Visual Question Answering, Safety-Filtered Trace Variant | Qwen3.5-122B-A10B | 
| Camio Chinese Question Answering, Verified-Correct Subset | Qwen3.5 | 
| KeenSight Chinese OCR, Filtered Subset | DeepSeek-OCR | 
| Camio Synthetic Trading-Graph Question Answering | Undisclosed | 
| Camio Synthetic Menu and Receipt Question Answering | Qwen3.5-122B-A10B | 
| Camio Chinese OCR, Formatted Variant | Qwen3.5-122B-A10B | 
| Camio Synthetic Time-Series Question Answering | Undisclosed | 
| Camio English OCR, Formatted Variant | Qwen3.5-122B-A10B | 
| Lean Formal Proofs — RL | Undisclosed | 
| Document-Grounded Finance Reasoning, Patch Tasks — RL | Undisclosed | 
| Math CoT — SFT | Kimi K3 | 
| Chat — SFT | Undisclosed | 
| Chat, Reasoning Off — SFT | Undisclosed | 
| Conversational Tool-Use Simulation — RL | Undisclosed | 
| Math CoT, Low Variant — SFT | Kimi K3 | 
| Document-Grounded Finance Reasoning — SFT | Undisclosed | 
| Safety Preference v0.3 — RL | Undisclosed | 
| Response-Length Instruction Following, Reasoning Off — SFT | Undisclosed | 
| Math Python TIR, Low Variant — SFT | Kimi K3 | 
| Math Python TIR — SFT | Kimi K3 | 
| General-Purpose Tool Calling — SFT | Qwen3.5-397B-A17B; Gemma-4-31B-it | 
| Knowledge Agent — SFT | Undisclosed | 
| DataFunnel arXiv Science4 v1.0 — RL | DeepSeek-V4-Flash (problem extraction); GLM-5.2 (refinement); Kimi K3 and K3 V5 (verification and filtering); Super 3.5 r14 (difficulty profiling); Qwen3-235B-A22B-Instruct-2507-FP8 (reward judging) | 
| Structured Outputs v3 — RL | gpt-oss-120b | 
| Response-Length Instruction Following — SFT | Undisclosed | 
| Filtered CUDA Coding — SFT | Undisclosed | 
| Reasoning Gym — RL | Undisclosed | 
| Competitive Coding Challenges v2 — SFT | GLM-5.2 | 
| Multi-Turn Instruction Following, Reasoning Off — SFT | Undisclosed | 
| Multi-Turn Instruction Following — SFT | Undisclosed | 
| Instruction Following, Filtered RLHF Prompts — SFT | Undisclosed | 
| Citation Formatting — RL | gpt-oss-120b | 
| Long-Context General Data — SFT | Undisclosed | 
| Free-Form Formatting — RL | gpt-oss-120b | 
| Structured Outputs v4 — RL | Undisclosed | 
| Tool-Call Schema — RL | No model used for RL conversion; source-generation models undisclosed | 
| Competitive Coding, OCR Python — SFT | DeepSeek-V4-Pro | 
| Lean Formal Proofs — SFT | gpt-oss-120b; Goedel-Prover-V2-32B; Qwen2.5-32B-Instruct | 
| Citation Instruction Following — RL | Undisclosed | 
| Search Graph Walks — SFT | MiniMax-M2 | 
| General-Purpose Tool Calling, Reasoning Off — SFT | Undisclosed | 
| Knowledge Agent Tau3, Reasoning Off — SFT | Undisclosed | 
| ARC-AGI Puzzle Reasoning — SFT | Kimi-K2.5; Qwen3-235B-A22B-Thinking-2507; DeepSeek-V3.2; Qwen3-Next-80B-A3B-Thinking; GLM-4.5-Air-FP8; gpt-oss-120b; GLM-4.7-FP8; Ring-1T; Ministral-3-14B-Reasoning-2512 | 
| Calendar — RL | Undisclosed | 
| ACEProof — SFT | Undisclosed | 
| Long Translation — SFT | Undisclosed | 
| Agentless Software Engineering — SFT | Undisclosed | 
| Long-Context PDF Translation v1.0 — SFT | DeepSeek-V4-Flash (translation generation); GPT-5.6 Sol (filtering and judging) | 

## Private Non-publicly Accessible Datasets by NVIDIA

| Dataset | Model(s) used | 
|---|---|
| Simple Minesweeper | Undisclosed | 
| Simple Sudoku | Undisclosed | 
| Multitool Typewriter Hard | Undisclosed | 
| Machine Translation of News Commentary and TAUS Translation Memory | Undisclosed | 
| Machine Translation of STEM - | Qwen2.5-14B-Instruct | 
| Competitive Coding RL data from Nemotron Cascade | Undisclosed | 
| Long context RL | Undisclosed | 
| Single-step SWE RL for patch generation | Undisclosed | 
| OpenHands SWE | Undisclosed | 

## NVIDIA-Sourced Synthetic Datasets (Pre-Training)

| Dataset | Modality | Dataset Size | Seed Dataset | Model(s) used for generation | 
|---|---|---|---|---|
| Nemotron-Pretraining-Fact-Seeking | Text | 35.0B | FineWiki | Qwen3-30B-A3B-Instruct-2507 | 
| Nemotron-Pretraining-Legal | Text | 4.3B | CommonPile (caselaw_access_project_filtered); California Code of Regulations; Judicial Ethics Opinions; GLOBALCIT; CUAD; Nemotron Personas; ToSDR Terms of Service Corpus; CodeHima/TOS_Dataset; ContractNLI; CaseHOLD; Code of Federal Regulations; Canadian Case Law (subsets that allow commercial use) | Qwen3-235B-A22B-Thinking-2507 | 
| Nemotron-Pretraining-Formal-Logic | Text | 128M | Nemotron Personas | Qwen3-235B-A22B-Thinking-2507 | 
| Nemotron-Pretraining-Economics | Text | 73.4M | - | Qwen3-235B-A22B-Thinking-2507 | 
| Nemotron-Pretraining-Multiple-Choice | Text | 1.6B | MMLU Auxiliary Train | DeepSeek-V3; Qwen3-235B-A22B | 
| Nemotron-Pretraining-Code-Concepts | Text | 7.3B | - | gpt-oss-20b; gpt-oss-120b | 
| Nemotron-Pretraining-Unconditional-Algorithmic | Text | 196.5M | - | gpt-oss-120b; Qwen3-235B-A22B | 
| More Synthetic Tasks from DeepSeek-V3 and Qwen3-235B-A22B | Text | 1.1B | train splits of acp_bench; ai2_arc; babi; gsm8k; hendrycks_math; IFEval; MedText; mediqa_qa; mlqa; MMLU-Pro; mmlu-pro-plus; MMLU-ProX; nq_open; tinyGSM8k; truthful_qa; truthfulqa-multi; MATH-lighteval; mmlu; awesome-chatgpt-prompts; super_glue | DeepSeek v3; Qwen3-235B-A22B | 
| Synthetic Tasks from DeepSeek-V3 and Qwen3-235B-A22B | Text | 6.7B | train splits of Into the Unknown; AI2 ARC (AI2 Reasoning Challenge); BLiMP (Benchmark of Linguistic Minimal Pairs); CommonSenseQA; GLUE; HeadQA; Hendrycks Ethics; Memo Trap; modus-tollens; NeQA; pattern-matching-suppression; mastermind_24_mcq_random; mastermind_24_mcq_close; quote-repetition; redefine-math; Repetitive Algebra; sig-figs; MMLU-Pro; MC-TACO; MedConceptsQA; MMLU_dataset; OpenbooksQA; PIQA (Physical Interaction Question Answering); SocialIQA; SuperGLUE; tinyAI2_arc; tinyMMLU; tinyWinogrande; TruthfulQA; WebQuestions; Winogrande; GPQA; MBPP | DeepSeek v3; Qwen3-235B-A22B | 
| Synthetic Art of Problem Solving from DeepSeek-R1 | Text | 40B | Art of Problem Solving; American Mathematics Competitions 8; American Mathematics Competitions 10 | DeepSeek-R1 | 
| Synthetic Moral Stories and Social Chemistry from Qwen3-235B-A22B-Thinking-2507 and Mixtral-8x22B-v0.1 | Text | 15.2M | social-chemestry-101; Moral Stories | Qwen3-235B-A22B-Thinking-2507; Mixtral-8x22B-v0.1 | 
| Synthetic Moral Stories and Social Chemistry from Mixtral-8x22B-v0.1 | Text | 327M | social-chemestry-101; Moral Stories | Mixtral-8x22B-v0.1 | 
| Synthetic Social Sciences seeded with OpenStax from DeepSeek-V3, Mixtral-8x22B-v0.1, and Qwen2.5-72B | Text | 83.6M | OpenStax - CC BY-SA subset | DeepSeek-V3; Mixtral-8x22B-v0.1; Qwen2.5-72B | 
| Synthetic Health Sciences seeded with OpenStax from DeepSeek-V3, Mixtral-8x22B-v0.1, and Qwen2.5-72B | Text | 9.7M | OpenStax - CC BY-SA subset | DeepSeek-V3; Mixtral-8x22B-v0.1; Qwen2.5-72B | 
| Synthetic STEM seeded with OpenStax, Open Textbook Library, and GSM8K from DeepSeek-R1, DeepSeek-V3, DeepSeek-V3-0324, and Qwen2.5-72B | Text | 175M | OpenStax - CC BY-SA subset; GSM8K; Open Textbook Library - CC BY-SA & GNU subset | DeepSeek-R1, DeepSeek-V3; DeepSeek-V3-0324; Qwen2.5-72B | 
| Nemotron-PrismMath | Text | 4.6B | Big-Math-RL-Verified; OpenR1-Math-220k | Qwen2.5-0.5B-instruct, Qwen2.5-72B-Instruct; DeepSeek-R1-Distill-Qwen-32B | 
| Synthetic Question Answering Data from Papers and Permissible Books from Qwen2.5-72B-Instruct | Text | 350M | arXiv; National Institutes of Health ExPorter; BioRxiv; PMC Article; USPTO Backgrounds; peS2o; Global Regulation; CORE; PG-19; DOAB CC BY & CC BY-SA subset; NDLTD | Qwen2.5-72B-Instruct | 
| Synthetic Rephrased Math Data from Common Crawl from phi-4 | Text | 73B | Common Crawl | phi-4 | 
| Synthetic Math Data from Common Crawl 4plus | Text | 52.3B | Common Crawl | phi-4 | 
| Synthetic Math Data from Common Crawl 3 | Text | 80.9B | Common Crawl | phi-4 | 
| Synthetic AGIEval seeded with AQUA-RAT, LogiQA, and AR-LSAT from DeepSeek-V3 and DeepSeek-V3-0324 | Text | 4.0B | AQUA-RAT; LogiQA; AR-LSAT | DeepSeek-V3; DeepSeek-V3-0324 | 
| Synthetic AGIEval seeded with AQUA-RAT, LogiQA, and AR-LSAT from Qwen3-30B-A3B | Text | 4.2B | AQUA-RAT; LogiQA; AR-LSAT | Qwen3-30B-A3B | 
| Synthetic Art of Problem Solving from Qwen2.5-32B-Instruct, Qwen2.5-Math-72B, Qwen2.5-Math-7B, and Qwen2.5-72B-Instruct | Text | Undisclosed | Art of Problem Solving; American Mathematics Competitions 8; American Mathematics Competitions 10; GSM8K; PRM800K | Qwen2.5-32B-Instruct; Qwen2.5-Math-72B; Qwen2.5-Math-7B; Qwen2.5-72B-Instruct | 
| Synthetic MMLU Auxiliary Train from DeepSeek-R1 | Text | 0.5B | MMLU Auxiliary Train | DeepSeek-R1 | 
| Synthetic Long Context Continued Post-Training Data from Papers and Permissible Books from Qwen2.5-72B-Instruct | Text | Undisclosed | arXiv; National Institutes of Health ExPorter; BioRxiv; PMC Article; USPTO Backgrounds; peS2o; Global Regulation; CORE; PG-19; DOAB CC BY & CC BY-SA subset; NDLTD | Qwen2.5-72B-Instruct | 
| Synthetic Common Crawl from Qwen3-30B-A3B and Mistral-Nemo-12B-Instruct | Text | 415.8B | Common Crawl | Qwen3-30B-A3B; Mistral-NeMo-12B-Instruct | 
| Synthetic Multilingual Data from Common Crawl from Qwen3-30B-A3B | Text | Undisclosed | Common Crawl | Qwen3-30B-A3B | 
| Synthetic Multilingual Data from Wikimedia from Qwen3-30B-A3B | Text | Undisclosed | Wikimedia | Qwen3-30B-A3B | 
| Synthetic Math Data from Wikimedia from Nemotron-4-340B-Instruct | Text | Undisclosed | - | Nemotron-4-340B-Instruct | 
| Synthetic Common Crawl Code from phi-4 | Text | 427.9B | Common Crawl | phi-4 | 
| Synthetic Scientific Coding from Qwen3-235B-A22B | Text | 1.2B | Wikimedia | Qwen3-235B-A22B | 
| Tool Calling Data | Text | 26.2B | - | Qwen3-235B-A22B-2507; gpt-oss-120b | 
| Synthetic Essential-Web from QwQ-32B | Text | 28.1B | Essential-Web | QwQ-32B | 
| Translated Synthetic Crawl | Text | 389.9B | Common Crawl | Qwen3-30B-A3B | 
| Translated Synthetic Wikipedia | Text | 7.9B | Wikimedia | Qwen3-30B-A3B | 
| Synthetic Long Context from Qwen3-235B-A22B-Instruct-2507 | Text | Undisclosed | CORE; PG-19; DOAB CC BY & CC BY-SA subset; NDLTD | Qwen3-235B-A22B-Instruct-2507 | 
| Synthetic Search STEM OPENQ from DeepSeek-R1-0528 | Text | Undisclosed | - | DeepSeek-R1-0528 | 
| Synthetic MCQ from Qwen2.5-32B-Instruct and DeepSeek-R1-0528 | Text | Undisclosed | - | Qwen2.5-32B-Instruct; DeepSeek-R1-0528 | 
| Synthetic Offline Search MCQA HLE from DeepSeek-R1-0528 | Text | Undisclosed | - | DeepSeek-R1-0528 | 
| Synthetic Offline Search MCQA GPQA from Qwen3-235B-A22B and DeepSeek-R1-0528 | Text | Undisclosed | - | Qwen3-235B-A22B; DeepSeek-R1-0528 | 
| Synthetic Human Preference from QwQ-32B, Qwen3-30B-A3B, Qwen3-235B-A22B, Qwen3-235B-A22B-Instruct-2507, Mistral-Small-3.1-24B-Instruct-2503, Mistral-Small-3.2-24B-Instruct-2506, MiniMax-M1-80k, MiniMax-M1-40k, Kimi-K2-Instruct, DeepSeek-V3-0324, DeepSeek-R1-0528 | Text | Undisclosed | - | QwQ-32B; Qwen3-30B-A3B; Qwen3-235B-A22B; Qwen3-235B-A22B-Instruct-2507; Mistral-Small-3.1-24B-Instruct-2503; Mistral-Small-3.2-24B-Instruct-2506; MiniMax-M1-80k; MiniMax-M1-40k; Kimi-K2-Instruct; DeepSeek-V3-0324; DeepSeek-R1-0528 | 
| Synthetic WildChat-1M and arena-human-preference-140k from DeepSeek-R1, gemma-2-2b-it, gemma-3-27b-it, gpt-oss-20b, gpt-oss-120b, Mistral-7B-Instruct-v0.3, Mixtral-8x22B-Instruct-v0.1, Nemotron-4-340B-Instruct, NVIDIA-Nemotron-Nano-9B-v2, Phi-4-mini-instruct, Phi-3-small-8k-instruct, Phi-3-medium-4k-instruct, Qwen3-235B-A22B, QwQ-32B | Text | Undisclosed | WildChat-1M; arena-human-preference-140k | DeepSeek-R1; gemma-2-2b-it; gemma-3-27b-it; gpt-oss-20b; gpt-oss-120b; Mistral-7B-Instruct-v0.3; Mixtral-8x22B-Instruct-v0.1; Nemotron-4-340B-Instruct; NVIDIA-Nemotron-Nano-9B-v2; Phi-4-mini-instruct; Phi-3-small-8k-instruct; Phi-3-medium-4k-instruct; Qwen3-235B-A22B; QwQ-32B | 
| Synthetic Code from Qwen3-32B | Text | Undisclosed | English Common Crawl; English Common Crawl 1.1 | Qwen3-32B | 
| Synthetic OpenCodeReasoning from DeepSeek-R1 | Text | Undisclosed | OpenCodeReasoning | DeepSeek-R1 | 
| Synthetic OpenCodeReasoning from DeepSeek-R1-0528 | Text | Undisclosed | OpenCodeReasoning | DeepSeek-R1-0528 | 
| Synthetic HackerRank Coding from DeepSeek-R1-0528 | Text | Undisclosed | HackerRank Coding Dataset | DeepSeek-R1-0528 | 
| Synthetic LIMO from DeepSeek-R1-0528 | Text | Undisclosed | LIMO | DeepSeek-R1-0528 | 
| Synthetic SCP from DeepSeek-R1-0528 | Text | Undisclosed | SCP-116K | DeepSeek-R1-0528 | 
| Synthetic Stack Exchange from DeepSeek-R1-0528 | Text | Undisclosed | Stack Exchange | DeepSeek-R1-0528 | 
| Synthetic Stack Exchange from gpt-oss-120b and Qwen2.5-32B-Instruct | Text | Undisclosed | Stack Exchange | gpt-oss-120b; Qwen2.5-32B-Instruct | 
| Synthetic Stack Exchange from gpt-oss-120b | Text | Undisclosed | Stack Exchange | gpt-oss-120b | 
| Synthetic Art of Problem Solving from gpt-oss-120b and Qwen2.5-32B-Instruct | Text | Undisclosed | Art of Problem Solving; American Mathematics Competitions 8; American Mathematics Competitions 10 | gpt-oss-120b; Qwen2.5-32B-Instruct | 
| Synthetic Common Crawl from Qwen3-30B-A3B | Text | Undisclosed | Common Crawl | Qwen3-30B-A3B | 
| Synthetic Wikipedia from Qwen3-30B-A3B | Text | Undisclosed | Wikimedia | Qwen3-30B-A3B | 
| Synthetic Essential-Web from Qwen3-30B-A3B and Qwen3-235B-A22B-Thinking-2507 | Text | Undisclosed | Essential-Web | Qwen3-30B-A3B; Qwen3-235B-A22B-Thinking-2507 | 
| Synthetic Essential-Web from gpt-oss-120b | Text | Undisclosed | Essential-Web | gpt-oss-120b | 
| Synthetic Textbook Math from Qwen3-30B-A3B, Qwen3-235B-A22B, phi-4 | Text | Undisclosed | Common Crawl; FineMath | Qwen3-30B-A3B; Qwen3-235B-A22B; phi-4 | 
| Synthetic Math and Code from DeepSeek-R1 and DeepSeek-R1-0528 | Text | Undisclosed | Magicoder-Evol-Instruct-110K; opc-sft-stage2; TACO; OpenCodeReasoning; OpenMathReasoning; NuminaMath CoT | DeepSeek-R1; DeepSeek-R1-0528 | 
| Synthetic Math from gpt-oss-120b and Qwen2.5-32B-Instruct | Text | Undisclosed | - | gpt-oss-120b; Qwen2.5-32B-Instruct | 
| Synthetic OpenMathReasoning from gpt-oss-120b and Qwen2.5-32B-Instruct | Text | Undisclosed | OpenMathReasoning | gpt-oss-120b; Qwen2.5-32B-Instruct | 
| Synthetic KernelBook from DeepSeek-R1-0528 | Text | Undisclosed | KernelBook | DeepSeek-R1-0528 | 
| Synthetic Scale HLE from gpt-oss-120b | Text | Undisclosed | Scale HLE | gpt-oss-120b | 
| Synthetic CDQuestions from gpt-oss-120b | Text | Undisclosed | CDQuestions | gpt-oss-120b | 
| Synthetic GPQA from gpt-oss-120b and Qwen2.5-32B-Instruct | Text | Undisclosed | Stack Exchange | gpt-oss-120b; Qwen2.5-32B-Instruct | 
| Synthetic Vedantu from gpt-oss-120b | Text | Undisclosed | Vedantu | gpt-oss-120b | 
| Synthetic Search STEM MCQ from Qwen3-235B-A22B and DeepSeek-R1-0528 | Text | Undisclosed | - | Qwen3-235B-A22B; DeepSeek-R1-0528 | 
| Synthetic OpenSTEM from Qwen2.5-32B-Instruct and DeepSeek-R1-0528 | Text | Undisclosed | - | Qwen2.5-32B-Instruct; DeepSeek-R1-0528 | 
| Synthetic MCQ10 from DeepSeek-R1-0528 | Text | Undisclosed | - | DeepSeek-R1-0528 | 
| Synthetic MCQ4 from Qwen3-235B-A22B, DeepSeek-R1-0528, and Qwen3-235B-A22B-Instruct-2507 | Text | Undisclosed | - | Qwen3-235B-A22B; DeepSeek-R1-0528; Qwen3-235B-A22B-Instruct-2507 | 

## NVIDIA-Sourced Synthetic Datasets (Post-Training)

| Dataset | Modality | Dataset Size | Seed Dataset | Model(s) used for generation | 
|---|---|---|---|---|
| Synthetic Single-Step Tool Use — RL | Text | Approximately 1,433,203 samples | Handwritten prompts; no external seed data | DeepSeek-V3.2; DeepSeek-R1-0528; Qwen3-235B-A22B-Thinking-2507; Qwen3-32B | 
| Synthetic Sparse Tables with Mixed Prompts | Text and images | 100,000 records | Generated in-house; no external seed data | None; generated without an SDG model | 
| Synthetic Single-Step Tool Use — RL | Text | Approximately 1,433,203 samples | Handwritten prompts; no external seed data | DeepSeek-V3.2; DeepSeek-R1-0528; Qwen3-235B-A22B-Thinking-2507; Qwen3-32B | 
| Synthetic Sparse Tables with Mixed Prompts | Text and images | 100,000 records | Generated in-house; no external seed data | None; generated without an SDG model | 
| Synthetic Competitive MATH Proofs from DeepSeek-V4-Pro | Text | Undisclosed | [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions] | [deepseek-ai/DeepSeek-V4-Pro] | 
| Synthetic Hermes Agent Reasoning Traces | Text | Undisclosed | [lambda/hermes-agent-reasoning-traces] | [hermes-agent-generator] | 
| Synthetic Competitive Coding from DeepSeek-V4-Pro | Text | Undisclosed | [NVCompetitiveCodingV1] | [deepseek-ai/DeepSeek-V4-Pro] | 
| Synthetic Competitive Science Reasoning from DeepSeek-V4-Pro | Text | Undisclosed | [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [EssentialAI/essential-web-v1.0]; [cdquestions.com]; [Pile-FreeLaw]; [Vedantu]; [askfilo]; [doubtnut]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)]; [AAPT]; [ChemData 700K]; [oMeBench]; [Flavor Analysis and Recognition Transformer]; [ChemCoTBench]; [Llama Nemotron Dataset] | [deepseek-ai/DeepSeek-V4-Pro] | 
| Synthetic Competitive MATH CoT and TIR from Nemotron 5.5 | Text | Undisclosed | [Pile-FreeLaw]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions] | [Nemotron 5.5] | 
| Vendor Terminal Bench-like Tasks from Mercor | Text | Undisclosed | [Terminal bench like tasks curated by the vendor] | [Undisclosed - purchased dataset] | 
| Turing Math Data Pack | Text | Undisclosed | [Turing Math Data Pack dataset] | [Undisclosed - purchased dataset] | 
| Synthetic Holdout, Skywork, DAPO, and Turing Math from GPT-5.5 | Text | Undisclosed | [DocQA-RL-1.6K]; [DAPO-Math-17k] | [GPT-5.5] | 
| Synthetic Long Context RL from QwenLong L1 and DocQA-RL-1.6K | Text | Undisclosed | [DocQA-RL-1.6K] | Undisclosed | 
| Synthetic Competitive Coding Gym Tasks | Text | Undisclosed | [NVCompetitiveCodingV1.1] | Undisclosed | 
| Synthetic Finance SEC Search Agent from GPT-OSS-120B and Qwen3 | Text | Undisclosed | [SEC filings from sec.gov] | [GPT-OSS-120B]; [Qwen3-235B-A22B-Instruct]; [Qwen3-4B-Instruct] | 
| Synthetic Structured Outputs from Qwen3-30B-A3B-Instruct-2507, Qwen3-30B-A3B-Thinking-2507, Qwen3-235B-A22B-Instruct-2507, and Qwen3-235B-A22B-Thinking-2507 | Text | Undisclosed | [Nemotron-RL-agent-structured-outputs-v1] | [Qwen3-30B-A3B-Instruct-2507]; [Qwen3-235B-A22B-Instruct-2507] | 
| Synthetic Long Context Equivalence Rule from Qwen3-235B-A22B-Thinking-2507 and DeepSeek-R1 | Text | Undisclosed | [Long-context SFT data] | [Qwen/Qwen3-235B-A22B-Thinking-2507]; [Deepseek-ai/DeepSeek-R1] | 
| Synthetic Science RL Data Blend from Qwen2.5-32B | Text | Undisclosed | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [Qwen2.5-32B] | 
| Synthetic Abstention Data from Nemotron Super v3 | Text | Undisclosed | [Go abstention Dataset] | [nvidia/nvidia/nemotron-3-super-v3] | 
| Synthetic Chemistry Data from Nemotron Super v3 | Text | Undisclosed | [ChemData 700K] | [nvidia/nvidia/nemotron-3-super-v3] | 
| Synthetic Tool Call Schema for RL | Text | 469,983 | [UltraTool]; [ToolEyes]; [AutoTools]; [API-Bank]; [Nemotron-Personas-USA]; [Salesforce xLAM function-calling]; [Glaive function-calling-v2]; [Agent-Ark/Toucan-1.5M] | [DeepSeek-V3.2]; [GLM-4.6]; [gpt-oss-120b]; [Kimi-K2-Instruct] | 
| Synthetic Freeform Text Formatting from GPT-OSS-120B | Text | Undisclosed | [In-house data] | [GPT OSS 120B - Apache 2.0] | 
| Synthetic Citation Formatting from GPT-OSS-120B | Text | Undisclosed | [In-house data] | [GPT OSS 120B - Apache 2.0] | 
| Droid Harness Pivot Vendor Data | Text | Undisclosed | [Droid Harness Pivot vendor data] | Undisclosed | 
| Synthetic HotpotQA Training Data from Qwen3-235B | Text | Undisclosed | [HotpotQA] | [Qwen3-235B] | 
| Synthetic Natural Language Math Proofs from Nemotron 5.5 | Text | Undisclosed | [AMC8, AMC10, and AIME problem sets hosted on Art of Problem Solving]; [Pile-StackExchange] | [Nemotron 5.5] | 
| Synthetic Stack Overflow OpenQ | Text | Undisclosed | [Pile-FreeLaw] | Undisclosed | 
| Chemistry Ether0 Vendor Data | Text | Undisclosed | [Chemistry ether0 vendor data] | Undisclosed | 
| Synthetic Litmus-Bench Chemistry from ChEMBL | Text | Undisclosed | [ChEMBL]; [Nemo Gym RL dataset generated from ChEMBL with RDKit] | Undisclosed | 
| Synthetic ZINC Chemistry from Nemotron Super v3 | Text | Undisclosed | [ZINC] | [Nemotron Super v3] | 
| ARC-AGI Gym Environment | Text | Undisclosed | [ARC-AGI-2] | [ARC-AGI-2] | 
| Synthetic Agentic Search Tool-Use from DeepSeek-V3.2 | Text | Undisclosed | [Mercor Data] | [DeepSeek-V3.2] | 
| Synthetic Text-To-SQL | Text | 96,564 | [In-house Text-to-SQL data] | [gpt-oss-120b] | 
| Dialog Memory Vendor Data | Text | Undisclosed | [Patronus external vendor agreement] | Undisclosed | 
| Synthetic Indirect Prompt Injection from Nemotron Super v3 and Qwen3-Next-80B-A3B-Instruct | Text | Undisclosed | [In-house indirect prompt injection data] | [nvidia/nemotron-3-super-v3, qwen/qwen3-next-80b-a3b-instruct] | 
| Synthetic Malicious Code and Agentic Security | Text | Undisclosed | [In-house malicious-code / agentic-security data] | Undisclosed | 
| Synthetic Single-Step SWE Patch Selection | Text | Undisclosed | [SWE-Gym Dataset]; [SWE Bench Verified Benchmark] | [ground truth and task checks] | 
| Synthetic Natural Language Math Final Answers from Nemotron 5.5 | Text | Undisclosed | [AMC8, AMC10, and AIME problem sets hosted on Art of Problem Solving]; [Pile-StackExchange] | [nemotron 5.5] | 
| Synthetic Simple Math Prompts for Token Efficiency | Text | Undisclosed | [In-house simple math prompts] | Undisclosed | 
| Synthetic Abstention Data from Nemotron Super v3 (CRAG) | Text | Undisclosed | [CRAG] | [nvidia/nvidia/nemotron-3-super-v3] | 
| Synthetic Agentless SWE | Text | 242,536 | [SWE-Rebench-V2]; [SWEbench Training Set]; [R2E-Gym/R2E-Gym-Subset]; [SWE-Gym/SWE-Gym]; [SWE-Rebench] | [openai/gpt-oss-120b] | 
| Synthetic Agentless SWE from DeepSeek-R1-0528 | Text | 209,976 | [SWE-Bench-Train]; [SWE-Fixer-Train]; [SWE-reBench]; [SWE-Smith] | [deepseek-ai/DeepSeek-R1-0528] | 
| Synthetic Agentic CUDA Traces from GLM-4.7 | Text | 2,276 | [Internal CUDA task data] | [GLM-4.7] | 
| Synthetic Math Proofs from DeepSeek-V3.2-Speciale | Text | 820,772 | [Nemotron-Math-Proofs-v1] | [SDG: DeepSeek-V3.2-Speciale]; [Filter: proof validation] | 
| Synthetic Multilingual SFT from DeepSeek-V3 | Text | 1,245,284 | [Nano v3 SFT data] | [DeepSeek-V3] | 
| Synthetic Agentic Code from gpt-oss-120b | Text | 109,086 | [NVAgenticCLIPrompts-v1]; [NVAgenticSkills-v1]; [NVAgenticCLIMultiTurnPrompts-v1] | [openai/gpt-oss-120b] | 
| Synthetic Agentic CLI and Web Skills from gpt-oss-120b | Text | 27,418 | [NVAgenticCLIPrompts-v1]; [NVAgenticSkills-v1]; [NVAgenticCLIMultiTurnPrompts-v1]; [NVAgenticCLIPrompts-Web-v1] | [openai/gpt-oss-120b] | 
| Synthetic Agentic Coding from gpt-oss-120b | Text | 160,531 | [NVAgenticCLIPrompts-v1]; [NVAgenticSkills-v1]; [NVAgenticCLIMultiTurnPrompts-v1]; [NVAgenticCLIPrompts-Web-v1] | [openai/gpt-oss-120b] | 
| Synthetic OpenCode Agentic Tasks from gpt-oss-120b | Text | 614,773 | [NVAgenticCLIPrompts-v1]; [NVAgenticSkills-v1]; [NVAgenticCLIMultiTurnPrompts-v1]; [NVAgenticCLIPrompts-Web-v1] | [openai/gpt-oss-120b] | 
| Synthetic SWE Unverified | Text | Undisclosed | [NVAgenticCLIPrompts-v1]; [NVAgenticSkills-v1]; [NVAgenticCLIMultiTurnPrompts-v1]; [NVAgenticCLIPrompts-Web-v1] | [gpt-oss-120b]; [Qwen/Qwen3-Coder-480B-A35B-Instruct]; [GLM-4.7-Flash] | 
| Synthetic ARC-AGI Ultra Data | Text | 192,016 | [ARC-AGI-2]; [arc dataset collection] | [ARC-AGI-2] | 
| Synthetic LiveCodeBench TIR from DeepSeek-R1-0528 | Text | 1,283,398 | [Nemotron-X training datasets] | [DeepSeek-R1-0528] | 
| Synthetic Verilog and SystemVerilog Code from DeepSeek-R1-0528 and GPT-OSS-120B | Text | 1,233,247 | [Verilog/SystemVerilog seed code] | [SDR: DeepSeek R1 0528 and GPT-OSS-120B]; [Filtering: Claude 4 Sonnet] | 
| Synthetic Aider Python Tasks from DeepSeek-R1-0528 | Text | 236,099 | [Exercism (GitHub Python)] | [Deepseek R1 0528] | 
| Synthetic Chat Reasoning-Off Data from GLM-5 | Text | 646,738 | [lmarena-ai/repochat-arena-preference-4k user prompts] | [Multi-turn conversations generated by GLM-5 with best-of-4 selection via Qwen3-Nemotron-235B-A22B-GenRM] | 
| Synthetic Chat Reasoning-On Data from GLM-5 | Text | 644,286 | [lmarena-ai/repochat-arena-preference-4k user prompts]; [lmarena-ai/arena-expert-5k user prompts]; [lmarena-ai/arena-human-preference-55k user prompts]; [lmarena-ai/arena-human-preference-100k user prompts]; [lmarena-ai/arena-human-preference-140k user prompts] | [Multi-turn conversations generated by GLM-5 with best-of-4 selection via Qwen3-Nemotron-235B-A22B-GenRM] | 
| Synthetic Multilingual Safety from Riva-Translate-4B-Instruct-v1.1 | Text | 132,067 | [Safety SFT Data: Ultra] | [nvidia/Riva-Translate-4B-Instruct-v1.1] | 
| Synthetic Science Reasoning Effort Medium | Text | 502,722 | [science-reasoning-effort-medium-v0] | Undisclosed | 
| Synthetic Telecom Tool-Use Trajectories from gpt-oss-120b | Text | 12,455 | [Existing Tau2 telecom trajectories originally generated with DeepSeek V3.2] | [gpt-oss-120b] | 
| Synthetic Terminal Bench Data from OpenReasoningv2 | Text | Undisclosed | [OpenCodeReasoningv2]; [OpenMathReasoning]; [nemo-swe-bench-repos]; [SWE-Rebench]; [SWE-Fixer-110K] | [OpenReasoningv2] | 
| Synthetic Tulu Instruction Following from DeepSeek-R1-0528 | Text | 105,361 | [Nemotron-X training datasets] | [DeepSeek-R1-0528] | 
| Synthetic Instruction Following from gpt-oss-120b | Text | 151,988 | [IFEval]; [IFEvalG] | [gpt-oss-120b] | 
| Synthetic Instruction Following for RL | Text | Undisclosed | [WildChat-1M]; [LMSYS-340B-Eval Dataset]; [LMSYS-Chat-1M Prompts]; [IFEval]; [IFEvalG] | [Qwen/Qwen3-235B-A22B-Thinking-2507]; [gpt-oss-120b]; [Qwen3-235B-A22B-Instruct-2507] | 
| Synthetic Identity Data from Qwen3-Next-80B-A3B-Instruct and Qwen3-235B-A22B-Instruct-2507 | Text | 25,992 | [Hand-written prompts] | [Qwen3-Next-80B-A3B-Instruct]; [Qwen3-235B-A22B-Instruct-2507] | 
| Synthetic Terminus Ultra Agentic Reasoning Blend | Text | 96,881 | [ARC-AGI-2]; [OpenCodeReasoningv2]; [OpenMathReasoning]; [SWE-Fixer-110K]; [SWE-Rebench]; [SWE-Smith] | [DeepSeek-V3.2]; [Qwen3-235B-A22B-Thinking-2507]; [Ring-1T]; [Kimi-K2.5]; [GLM-4.7-FP8]; [Qwen3-Next-80B-A3B-Thinking]; [gpt-oss-120b]; [Ministral-3-14B-Reasoning-2512]; [LM-4.5-Air-FP8] | 
| Synthetic STEM from Qwen3-235B-A22B-Thinking-2507 | Text | 1,174,694 | [IChO-IPhO-RL-v2]; [Physics-Big Dataset]; Scale HLE; [OpenMathReasoning]; [OpenCodeReasoning] | [Qwen3-235B-A22B-Thinking-2507] | 
| Synthetic STEM from Qwen3-235B-A22B-Instruct-2507 and gpt-oss-120b | Text | Undisclosed | [arXiv]; [National Institutes of Health ExPorter]; [BioRxiv]; [PMC Article]; [USPTO Backgrounds]; [peS2o]; Global Regulation; [CORE]; [PG-19]; [DOAB CC BY & CC BY-SA subset]; [NDLTD] | [Qwen3-235B-A22B-Instruct-2507]; [gpt-oss-120b] | 
| Translation Data from TAUS | Text | 1,618,055 | [TAUS proprietary dataset] | Undisclosed | 
| Synthetic Art of Problem Solving and Stack Exchange from gpt-oss-120b, Qwen2.5-32B-Instruct, and Goedel-Prover-V2-32B | Text | 860,469 | [Nemotron-Math-Proofs-v1] | [Goedel-Prover-V2-32B] | 
| Synthetic Art of Problem Solving and Stack Exchange from gpt-oss-120b | Text | 1,201,815 | [Upstream released math dataset]; [AoPS]; [StackOverflow / StackExchange] | [gpt-oss-120b] | 
| Synthetic Multilingual Science and Code data from DeepSeek-R1, DeepSeek-R1-0528, Qwen2.5-32B-Instruct, and Qwen3-235B-A22B, translated with Qwen2.5-32B-Instruct and Qwen2.5-14B-Instruct | Text | Undisclosed | [Nano-V3 SFT Data (without tool call)] | [Qwen/Qwen2.5-14B-Instruct]; [Qwen/Qwen3-4B-Thinking-2507] | 
| Synthetic Multilingual Science and Code data from DeepSeek-R1, DeepSeek-R1-0528, Qwen2.5-32B-Instruct, and Qwen3-235B-A22B, translated with Qwen2.5-32B-Instruct and Qwen2.5-14B-Instruct (Stack Exchange lineage) | Text | Undisclosed | [Stack Exchange]; [SCP-116K]; [LIMO]; [TACO]; Code Contest; Codeforces | [DeepSeek-R1]; [DeepSeek-R1-0528]; [Qwen2.5-32B-Instruct]; [Qwen3-235B-A22B] | 
| Synthetic Search Graph Walk | Text | 6,977 | [Wikidata / Wikipedia KnowledgeBase] | [MiniMaxAI/MiniMax-M2] | 
| Synthetic Agentic Diverse Domains | Text | 281,537 | [Handwritten prompts (synthetic; no external seed data used)] | [SDG model: deepseek-ai/DeepSeek-V3.2, deepseek-ai/DeepSeek-R1-0528, Qwen/Qwen3-235B-A22B-Thinking-2507, Qwen/Qwen3-32B]; [Filtering model: openai/gpt-oss-120b, Qwen/Qwen3-32B, Qwen/Qwen3-235B-A22B-Instruct-2507] | 
| Synthetic Long Context from Qwen3-235B-A22B-Instruct-2507 | Text | Undisclosed | [Long-context SFT seed blend (pre-training blend + nano-v1 post-training data)]; [Long-context SFT data: lc_nothink 256k, MRCR 200k, RULER 256k]; [AALCR seed blend: SEC Filings, CC, Wikipedia, FinePDFs, ArXiv, Pile-NIH ExPorter, BioRxiv, PMC Article, USPTO Backgrounds, peS2o, Global Regulations, CORE, Gutenberg (PG-19), DOAB CC-BY, NDLTD, Amps, StackExchange, MathPile, Numinas] | [Qwen/Qwen3-235B-A22B-Thinking-2507]; [deepseek-ai/DeepSeek-R1]; [Qwen3-30B-A3B] | 
| Synthetic Nemotron Math SFT from DeepSeek-V3.2-Speciale | Text | 1,900,553 | [Nemotron-Math-v2 (AOPS and StackExchange-math problems)] | [DeepSeek-V3.2-Speciale] | 
| Synthetic Nemotron Math TIR from DeepSeek-V3.2 | Text | 1,789,258 | [Nemotron-Math-v2 (AOPS and StackExchange-math problems)] | [DeepSeek-V3.2] | 
| Synthetic NemoCascade OCR Distillation from gpt-oss-120b | Text | 682,864 | [Nemotron-X training datasets] | [gpt-oss-120b] | 
| Synthetic CUDA 100k | Text | 93,086 | [KernelBook]; [HuggingFace Transformers]; [FlashInfer] | [gpt-oss-120b]; [DeepSeek-R1-0528] | 
| Synthetic Science MCQ and QA Diversity from GPT-OSS and Kimi-K2 | Text | 30,358 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Science HLE with Python from GPT-OSS and Kimi-K2 | Text | 85,184 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Science Search and Python from GPT-OSS and Kimi-K2 | Text | 6,179 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Science Search from GPT-OSS and Kimi-K2 | Text | 32,554 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Finance Reasoning from GPT-OSS-120B and Qwen3-235B-A22B-Instruct-2507 | Text | 326,700 | [SEC filings] | [GPT-OSS-120B, Qwen3-235B-A22B-Instruct-2507] | 
| Synthetic Science Diversity MCQ from GPT-OSS and Kimi-K2 | Text | 532,942 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Science Diversity OpenQ from GPT-OSS and Kimi-K2 | Text | 131,045 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Science Reasoning No-Tool from GPT-OSS and Kimi-K2 | Text | 2,085,600 | [doubtnut]; [Pile-FreeLaw]; [Llama Nemotron Dataset]; [askfilo]; [EssentialAI/essential-web-v1.0]; [Vedantu]; [auxiliary_train]; [cdquestions.com]; [AMC 8 Problems and Solutions, AMC 10 Problems and Solution, and AIME Problems and Solutions]; [AAPT]; [ICHO-IPH0 Dataset]; [LIMO dataset (Less is More for Reasoning)] | [GPT-OSS]; [Kimi-K2] | 
| Synthetic Text-To-SQL from gpt-oss-120b | Text | Undisclosed | [In-house Text-to-SQL data] | [gpt-oss-120b] | 
| Synthetic Tool Call Schema for RL (extended) | Text | 707,967 | [UltraTool]; [ToolEyes]; [AutoTools]; [API-Bank]; [Nemotron-Personas-USA]; [Salesforce xLAM function-calling]; [Glaive function-calling-v2]; [Agent-Ark/Toucan-1.5M] | [DeepSeek-V3.2]; [GLM-4.6]; [gpt-oss-120b]; [Kimi-K2-Instruct] | 
| Synthetic Safety from gemma-3-4b-it, Nemotron-Nano-9B-v2, and gpt-oss-120b | Text | 44,091 | [Safety SFT Data] | [google/gemma-3-4b-it]; [Nemotron-Nano-9B-v2]; [gpt-oss-120b] | 
| Synthetic Safety from DeepSeek-R1-0528, gpt-oss-120b, DeepSeek-R1-Distill-Qwen-7B, and Mixtral-8x7B-v0.1 | Text | Undisclosed | [Nemotron Content Safety Dataset V2]; [Gretel Synthetic Safety Alignment Dataset]; [RedTeam-2K]; [Malicious Tasks]; [Nemotron-Personas-USA] | [DeepSeek-R1-0528]; [gpt-oss-120b]; [DeepSeek-R1-Distill-Qwen-7B]; [Qwen3-30B-A3B-Thinking-2507]; [Qwen3-235B-A22B-Instruct-2507]; [Mixtral-8x7B-v0.1] | 
| Synthetic Tool Calling from Qwen3-235B-A22B-Thinking-2507 and Qwen3-Next-80B-A3B-Thinking | Text | Undisclosed | [ToolBench]; [glaive-function-calling-v2]; [APIGen Function-Calling]; [Nemotron-Personas-USA] | [Qwen3-235B-A22B-Thinking-2507]; [Qwen3-Next-80B-A3B-Thinking] | 
| Synthetic Chat from gpt-oss-120b, Mixtral-8x22B-Instruct-v0.1, Qwen3-235B-A22B-Instruct-2507, and Qwen3-235B-A22B-Thinking-2507 | Text | Undisclosed | [C4]; [LMSYS-Chat-1M]; [ShareGPT]; [GSM8K]; [PRM800K]; [FinQA]; [WikiTableQuestions]; [Riddles]; [glaive-function-calling-v2]; [SciBench]; [tigerbot-kaggle-leetcodesolutions-en-2k]; [OpenBookQA]; [Advanced Reasoning Benchmark]; Software Heritage; [Khan Academy Math Keywords]; [WildChat-1M]; [Nemotron-Personas-USA] | [gpt-oss-120b]; [Mixtral-8x22B-Instruct-v0.1]; [Qwen3-235B-A22B-Instruct-2507]; [Qwen3-235B-A22B-Thinking-2507] | 
| Synthetic Tool Use Interactive Agent from gpt-oss-120b, DeepSeek-R1-0528, Qwen3-32B, and Qwen3-235B-A22B-Thinking-2507 | Text | Undisclosed | NVIDIA Internal | [gpt-oss-120b]; [DeepSeek-R1-0528]; [Qwen3-32B]; [Qwen3-235B-A22B-Thinking-2507] | 
| Synthetic DocFinQA and SWE-smith from Qwen3-Coder-480B-A35B-Instruct and Kimi-K2-Thinking | Text | Undisclosed | [DocFinQA]; [SWE-smith] | [Qwen3-Coder-480B-A35B-Instruct]; [Kimi-K2-Thinking] | 
| Synthetic SWE-Gym from Qwen3-Coder-480B-A35B-Instruct | Text | Undisclosed | [SWE-Gym] | [Qwen3-Coder-480B-A35B-Instruct] | 
| Synthetic SWE-Gym and R2E-Gym-Subset from Qwen3-Coder-480B-A35B-Instruct | Text | Undisclosed | [SWE-Gym]; [R2E-Gym-Subset] | [Qwen3-Coder-480B-A35B-Instruct] | 
| Synthetic SWE-Gym and R2E-Gym-Subset from DeepSeek-R1-0528 | Text | Undisclosed | [SWE-Gym]; [R2E-Gym-Subset] | [DeepSeek-R1-0528] | 
| Synthetic HelpSteer, LMSYS-Chat-1M, and Nemotron-Personas-USA from gpt-oss-120b, Qwen3-235B-A22B-Instruct-2507, and Qwen3-235B-A22B-Thinking-2507 | Text | Undisclosed | [HelpSteer2]; [HelpSteer3]; [LMSYS-Chat-1M]; [Nemotron-Personas-USA] | [gpt-oss-120b]; [Qwen3-235B-A22B-Instruct-2507]; [Qwen3-235B-A22B-Thinking-2507] | 
| Synthetic Nemotron-Personas-USA from gpt-oss-120b and Qwen3-8B | Text | Undisclosed | [Nemotron-Personas-USA] | [gpt-oss-120b]; [Qwen3-8B] | 
| Vendor Terminal Bench-like Tasks (Droid) | Text | Undisclosed | [Droid Harness Pivot vendor data] | Undisclosed | 

## Language Distribution in Post-Training

For our post-training recipe, we focused on the following languages in addition to English: French, German, Italian, Japanese, Spanish, and Chinese. Those languages were represented in the form of multilingual reasoning and translation tasks.

### Additional Training Data

| Data | Composition and Purpose | 
|---|---|
| Indic continued pre-training | 100B tokens: 10B selected Indic tokens and 90B tokens from the preceding blend. | 
| Software-engineering mid-training | 67B tokens: whole repositories (20%), issue–pull-request pairs (41%), commit diffs (3%), build and CI logs (1%), agent traces (10%), pre-training replay (20%), and long-context data (5%). Sources are decontaminated against SWE-bench and Terminal-Bench repositories. | 
| Long-context pre-training | A 35B-token extension to 1M context, followed by a 9B-token long-context recovery stage. | 
| Vision-language data | Image and video data from the Nemotron 3 Nano Omni training corpus, including visual question answering, charts, tables, OCR, multi-page documents, visual grounding, and video understanding. | 

| Post-Training Data | Composition and Purpose | 
|---|---|
| Additional synthetic vision data | Chart understanding, grounded perception, multimodal reasoning, and visual tool-use trajectories, including data generated with Qwen3.5 and Kimi models. | 
| Reasoning trajectories | Mathematics and STEM responses generated with Kimi K3; mathematical proofs generated with DeepSeek V4 Pro; competitive-programming trajectories generated with GLM-5.2. | 
| Chat and safety | Refreshed chat responses from Qwen, Kimi, and Gemma models, with additional safety data. | 
| Long-context supervised data | 512K-context data, synthetic graph reasoning and transduction tasks, long-horizon search and coding trajectories, and documents containing up to hundreds of pages. | 
| Long-document translation | Chunked translations of PG19 and FinePDFs-Edu documents generated with DeepSeek V4 Flash. | 

## Testing Datasets

**Data Collection Method:** Hybrid: automated, manually-collected, and synthetic

**Labeling Method:** Hybrid: automated, manually-labeled, and synthetic

**Properties:** Standard benchmarks and task-specific test suites covering reasoning, coding, tool use, instruction following, long context, and visual understanding.

## Evaluation Datasets

**Data Collection Method:** Hybrid: automated, manually-collected, and synthetic

**Labeling Method:** Hybrid: automated, manually-labeled, and synthetic

**Properties:** Standard benchmarks and task-specific test suites covering reasoning, coding, tool use, instruction following, long context, and visual understanding.

## Inference

- **Acceleration Engine:** vLLM (NVIDIA NIM)
- **Test Hardware:**  - NVIDIA Ampere
    - 2x A100-80GB
  - NVIDIA Blackwell
    - 1-2x B200, 1-2x GB200
    - 1-2x B300, 1-2x GB300
    - 2x RTX Pro 6000 Blackwell Server Edition
    - DGX Station
    - DGX Spark
  - NVIDIA Hopper
    - 2-8x H100
    - 1-4x H200
- NVIDIA Ampere

## Ethical Considerations

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. When downloaded or used in accordance with our terms of service, developers should work with their internal model team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse.

We advise against circumvention of any provided safety guardrails contained in the Model without a substantially similar guardrail appropriate for your use case. For more details: Safety and Explainability subcards. For more detailed information on ethical considerations for this model, please see the Model Card++ Bias, and Privacy subcards.

Please report model quality, risk, security vulnerabilities or NVIDIA AI Concerns here.

## **Get Help**

### NVIDIA Developer Community Forum

Get access to community knowledge base articles and support cases (https://forums.developer.nvidia.com/)

### Enterprise Support

Get access to knowledge base articles and support cases or submit a ticket.
