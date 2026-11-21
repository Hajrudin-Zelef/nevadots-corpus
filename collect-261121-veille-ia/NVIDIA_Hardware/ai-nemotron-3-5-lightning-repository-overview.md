---
id: collect-261121-veille-ia/nvidia-hardware/ai-nemotron-3-5-lightning-repository-overview
title: "ai/nemotron-3.5-lightning repository overview"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "lightning", "h100", "nvidia-nemotron-3", "nemotron", "vllm", "nemotron-3"]
source: https://hub.docker.com/r/ai/nemotron-3.5-lightning
source_anchor: ""
source_lines: [1, 272]
section: "NVIDIA_Hardware"
---
*Unsloth Dynamic 2.0 achieves superior accuracy & outperforms other leading quants.*
  

| **Total Parameters** | 30B (3B active) | 
| **Architecture** | MoE — Mamba-2 + MoE + Attention hybrid | 
| **Precision** | BF16 (full-precision reference weights) | 
| **Context Length** | Up to 1M tokens (for single H100 deployment, we use 256K) | 
| **Single-GPU Deployment** | 1× H100 80GB (or 1× A100 80GB) | 
| **Supported Hardware** | NVIDIA Blackwell (GB200, GeForce RTX 5090); NVIDIA Hopper (H100, H200); NVIDIA Ampere (A100) | 
| **Supported Languages** | English (and coding languages), Spanish, French, German, Italian, Japanese | 
| **Speculative Decoding** | DSpark for Low Concurrency Data Centre Deployments — Read more below | 
| **Reasoning Mode** | Configurable on/off via chat template ( `enable_thinking=True/False` ) | 
| **Recommended Sampling** | Temperature 1.0, Top_P 0.95 | 
| **Best For** | Customization — post-training (SFT, RL, distillation), domain adaptation, building quantized variants, and research/evaluation at full precision | 
| **Looking to Deploy?** | For optimized inference, see NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4 | 
| **License** | OpenMDW License Agreement, version 1.1 | 
| **Release Date** | August 11, 2026 | 

**Model Developer:** NVIDIA Corporation

**Model Dates:** December 2025 - May 2026

**Data Freshness:**

NVIDIA Nemotron™ is a family of open models with open weights, training data, and recipes, delivering leading efficiency and accuracy for building specialized AI agents.

**NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16** is a large language model (LLM) trained by NVIDIA. This is the full-precision (BF16) release of Nemotron 3.5 Lightning — the reference weights of the model, intended primarily as the starting point for customization: post-training (SFT, RL, distillation), domain adaptation, and producing your own quantized or GGUF variants. For latency- and throughput-optimized inference, use the NVFP4 release instead.

The model employs a hybrid **Mixture-of-Experts** architecture, utilizing interleaved Mamba-2 and MoE layers, along with select Attention layers. The Lightning 3.5 model is released alongside a number of speculative decoding methods for faster text generation. The model has **3B active parameters** and **30B parameters in total**.

This model is ready for commercial use.

*For running Nemotron 3.5 Lightning fast — with NVFP4 quantization, W4A16 for broad hardware coverage, and the DSpark recipe for DGX Spark — please see: NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4*


To get quickly started on a single H100 you can use the following command.

Grab the model:

```
export MODEL_CKPT=nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16
```
Run it with vLLM! (vLLM Nightly: `vllm/vllm-openai:v0.27.1`)

```
vllm serve --model $MODEL_CKPT \
    --max-num-seqs 128 \
    --enable-prefix-caching \
    --async-scheduling \
    --mamba-backend flashinfer \
    --mamba-ssm-cache-dtype float16 \
    --enable-mamba-cache-stochastic-rounding \
    --mamba-cache-philox-rounds 5
```
For more details on how to deploy and use the model — see the Quick Start Guide below!

**Governing Download Terms:** Use of this model is governed by the OpenMDW-1.1 model license.

We evaluated our model on the following benchmarks:

| Task | Nemotron-3.5-Lightning-30B-A3B-BF16 | Qwen 3.6 35B A3B | Gemma 4 26B A4B | Nemotron 3 Nano | Nemotron 3 Super | GPT-OSS 20B | 
|---|---|---|---|---|---|---|
| **General Knowledge** |  |  |  |  |  |  | 
| MMLU Pro | 81.94 | **85.63** | 85.20 | 78.46 | 83.89 | 76.40 | 
| AA-Omniscience | 17.50 | 19.47 | 22.17 | 20.15 | **26.68** | 16.62 | 
| **Reasoning** |  |  |  |  |  |  | 
| GPQA Diamond (no tools) | 75.44 | **83.40** | 79.61 | 74.05 | 78.60 | 71.46 | 
| HLE (text-only, no tools) | 11.72 | 19.56 | 17.42 | 10.89 | **20.30** | 13.76 | 
| SciCode | 32.60 | 35.33 | **40.28** | 30.08 | 35.11 | 38.63 | 
| **Coding & Agentic** |  |  |  |  |  |  | 
| SWE-bench Verified | 51.56 | **70.12** | 57.40 | 34.08 | 63.08 | 52.44 | 
| SWE-bench Multilingual | 39.33 | **63.40** | 43.40 | 14.07 | 49.80 | 41.93 | 
| Terminal-Bench 2.1 | 24.58 | **44.38** | 37.22 | 8.29 | 39.61 | 15.17 | 
| PinchBench | 85.37 | **88.07** | 74.70 | 66.11 | 80.36 | 57.20 | 
| BrowseComp | 36.97 | **48.74** | 26.30 | 13.74 | 22.77 | – | 
| τ³-bench (Banking) | 9.28 | 10.52 | **14.02** | 7.01 | 12.37 | – | 
| GDPval-AA-V2 | 832 | **1015** | 807 | 473 | 746 | – | 
| **Instruction Following** |  |  |  |  |  |  | 
| IFBench (loose) | 71.88 | 63.71 | **77.25** | 72.17 | 71.92 | 68.50 | 
| **Long Context** |  |  |  |  |  |  | 
| AA-LCR | 52.00 | **61.06** | 57.56 | 32.75 | 58.44 | 32.88 | 

Accuracy numbers measured by NVIDIA under a consistent harness (NeMo Gym / Nemo Evaluator SDK); they may differ from vendors' self-reported numbers.

For reproducibility, the evaluation recipes, installation instructions, and commands for NVIDIA Nemotron 3.5 Lightning were collected and published in NeMo Gym. The reported results cover the release evaluation suite, including knowledge and reasoning, instruction following, coding, agentic, tool-use, and long-context. Most evaluations use NeMo Gym-native harnesses while a small subset, including SWE-Bench and Terminal-Bench, used NeMo Evaluator natively. The published recipes specify the benchmark-specific containers, prompts, inference parameters, parser configurations, and scoring settings used to produce the results.

Additional harness-level coding-agent results for SWE-Bench Verified and Terminal-Bench 2.1 are shown below.

NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16 is the full-precision release of a general purpose reasoning and chat model, and is intended primarily for **customization and post-training** rather than direct production inference. It is intended to be used by developers who want to: post-train the model on their own data (SFT, RL via NeMo RL and NeMo Gym, or distillation), adapt it to a domain or task, produce quantized variants (NVFP4, W4A16, GGUF) for their own deployment targets, or run full-precision research and evaluation. English and coding languages are the primary languages, with Spanish, French, German, Italian, and Japanese also supported.

For developers who want to *deploy* Lightning 3.5 directly — in AI agent systems, chatbots, RAG systems, and instruction-following applications — the NVFP4 release is the recommended path, with optimized recipes for data centre and DGX Spark deployments.

Hugging Face — 08/11/2026

The model was pre-trained with over 20T tokens and supports up to 1M context length. The pre-training phase used an NVFP4 recipe. The model includes **Multi-Token Prediction (MTP)** layers, which predict multiple future tokens to provide richer training signals.

Stage 1: Pre-Training

Stage 2: Continued Pre-Training for Multi-Token Prediction (MTP)

Stage 3: Supervised Fine-Tuning

Stage 4: Reinforcement Learning

NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16 is a result of the above work.

Our AI models are designed and/or optimized to run on NVIDIA GPU-accelerated systems. By leveraging NVIDIA's hardware (e.g. GPU cores) and software frameworks (e.g., CUDA libraries), the model achieves faster training and inference times compared to CPU-only solutions.

The integration of foundation and fine-tuned models into AI systems requires additional testing using use-case-specific data to ensure safe and effective deployment. Following the V-model methodology, iterative testing and validation at both unit and system levels are essential to mitigate risks, meet technical and functional requirements, and ensure compliance with safety and ethical standards before deployment.

All deployment snippets below assume:

And for DSpark:

```
export DSPARK_CKPT=nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4-DSpark
```
The BF16 recipes below cover vLLM. For TensorRT-LLM, SGLang, W4A16 (Blackwell / Hopper / Ampere from a single checkpoint), and DGX Spark (DSpark) recipes, see the NVFP4 card.


Lightning 3.5 ships with two external draft models for speculative decoding as well as MTP (Multi-Token Prediction). While we currently recommend DSpark for all cases - your usecase may align with DFlash and MTP:

`vllm/vllm-openai:v0.27.1`
For max throughput deployments, use the following configuration, no speculative decoding strategy is best for this serving configuration, and due to memory constraints the Mamba cache `dtype` is set as FP16:

```
vllm serve --model $MODEL_CKPT \
    --max-num-seqs 128 \
    --enable-prefix-caching \
    --async-scheduling \
    --mamba-backend flashinfer \
    --mamba-ssm-cache-dtype float16 \
    --enable-mamba-cache-stochastic-rounding \
    --mamba-cache-philox-rounds 5 \
    --reasoning-parser nemotron_v3 \
    --tool-call-parser qwen3_coder \
    --enable-auto-tool-choice
```
For long-context, multi-GPU serving (TP8 with expert parallelism):

```
VLLM_ALLOW_LONG_MAX_MODEL_LEN=1 vllm serve --model $MODEL_CKPT \
    --moe-backend flashinfer_cutlass \
    --mamba-backend flashinfer \
    --enable-prefix-caching \
    --mamba-cache-mode align \
    --max-model-len 1048576 \
    --enable-expert-parallel \
    --tensor-parallel-size 8 \
    --reasoning-parser nemotron_v3 \
    --tool-call-parser qwen3_coder \
    --enable-auto-tool-choice
```
```
VLLM_ALLOW_LONG_MAX_MODEL_LEN=1 vllm serve --model $MODEL_CKPT \
    --max-num-seqs 128 \
    --max-model-len 1048576 \
    --max-num-batched-tokens 10240 \
    --no-enable-prefix-caching \
    --async-scheduling \
    --speculative_config.model $DSPARK_CKPT \
    --speculative_config.num_speculative_tokens 5 \
    --mamba-backend flashinfer \
    --reasoning-parser nemotron_v3 \
    --tool-call-parser qwen3_coder \
    --enable-auto-tool-choice
```
`--max-model-len` to match your workload and drop `VLLM_ALLOW_LONG_MAX_MODEL_LEN=1`.
The examples below use the OpenAI-compatible client and work with the serving backend above. Recommended sampling settings are **Temperature 1.0** and **Top_P 0.95**.

The vLLM snippets above register the model as `nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16` via `--served-model-name`.

```
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="EMPTY")
MODEL = "nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16"
```
Lightning 3.5 exposes reasoning control through chat-template kwargs: thinking enabled (the default), thinking disabled for direct answers, and a runtime thinking budget.

**Reasoning ON (default)**

```
response = client.chat.completions.create(
    model=MODEL,
    messages=[{"role": "user", "content": "Write a haiku about GPUs"}],
    max_tokens=16000,
    temperature=1.0,
    top_p=0.95,
    extra_body={"chat_template_kwargs": {"enable_thinking": True}}
)
print(response.choices[0].message.content)
```
**Reasoning OFF**

```
response = client.chat.completions.create(
    model=MODEL,
    messages=[{"role": "user", "content": "What is the capital of Japan?"}],
    max_tokens=16000,
    temperature=1.0,
    top_p=0.95,
    extra_body={"chat_template_kwargs": {"enable_thinking": False}}
)
print(response.choices[0].message.content)
```
**Streaming**

```
stream = client.chat.completions.create(
    model=MODEL,
    messages=[{"role": "user", "content": "Explain speculative decoding in two sentences"}],
    max_tokens=16000,
    temperature=1.0,
    top_p=0.95,
    stream=True,
)
for chunk in stream:
    print(chunk.choices[0].delta.content or "", end="", flush=True)
```
For vLLM, add the following to any serve command above:

```
    --enable-auto-tool-choice \
    --tool-call-parser qwen3_coder \
    --reasoning-parser nemotron_v3
```
**NOTE:** For coding agents, add `extra_body={"chat_template_kwargs": {"force_nonempty_content": True}}` to the API call, as shown below.

```
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get the current weather for a city",
        "parameters": {
            "type": "object",
            "properties": {"city": {"type": "string"}},
            "required": ["city"],
        },
    },
}]
response = client.chat.completions.create(
    model=MODEL,
    messages=[{"role": "user", "content": "What's the weather in Santa Clara?"}],
    tools=tools,
    max_tokens=16000,
    temperature=1.0,
    top_p=0.95,
    extra_body={"chat_template_kwargs": {"force_nonempty_content": True}},
)
print(response.choices[0].message.tool_calls)
```
**Data Modality:** Text
**Training Data Size:** More than 20 Trillion Tokens
**Dataset partition:** *Training [100%], testing [0%], validation [0%]*
**Time period for training data collection:** 2013 to December 2025
**Time period for testing data collection:** 2013 to December 2025
**Time period for validation data collection:** 2013 to December 2025
**Data Collection Method by dataset:** Hybrid: Automated, Manually-Collected, Synthetic
**Labeling Method by dataset:** Hybrid: Automated, Manually-Labeled, Synthetic

NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16 is pre-trained on a large corpus of high-quality curated and synthetically-generated data. It is trained in the English language, as well as 19 other spoken languages and 43 programming languages. Our sources cover a variety of document types such as: webpages, dialogue, articles, and other written materials. The corpus spans domains including legal, math, science, finance, and more. We also include a small portion of question-answering, and alignment style data to improve model accuracy. The model was pre-trained for more than 20 trillion tokens.

The post-training corpus for NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16 consists of high-quality curated and synthetically-generated data. Primary languages used for post-training include English, French, German, Italian, Japanese, Spanish, and Chinese.

These datasets, such as FinePDFs, EssentialWeb, HotpotQA, SQuAD, and HelpSteer3, do not collectively or exhaustively represent all demographic groups (and proportionally therein). For instance, these datasets do not contain explicit mentions of demographic classes such as age, gender, or ethnicity in 64-99% of samples, depending on the source. In the subset where such terms are present, document-based datasets (FinePDFs and EssentialWeb) contain representational skews, such as references to "male" outnumbering those to "female", and mentions of "White" as the most frequent among ethnic identifiers (comprising 43-44% of ethnicity mentions). To mitigate these imbalances, we recommend considering evaluation techniques such as bias audits, fine-tuning with demographically balanced datasets, and mitigation strategies like counterfactual data augmentation to align with the desired model behavior. This evaluation used a 3,000-sample subset per dataset, identified as the optimal threshold for maximizing embedder accuracy.

During post-training, we generate synthetic data by distilling trajectories, solutions, and translations from strong teacher models and agent systems, often grounded in real tasks or documents and aggressively filtered for quality. For math, code, and science, we start from curated problem sets and use open source permissive models such as GPT-OSS-120B to produce step-by-step reasoning traces, candidate solutions, best-of-n selection traces, and verified CUDA kernels. For long-context and science, we build synthetic QA and reasoning data by retrieving passages from long documents, generating MCQ/OpenQA questions and answers, and paraphrasing them into multiple prompt/response formats to ensure diversity. Across all pipelines we stack automated verification—compilers, numerical checks, language identification—to ensure our data is high quality.

For all domains, we apply a unified data filtering pipeline to ensure that only high-quality, license-compliant, and verifiable samples are used for post-training. We first discard malformed examples using structural checks (e.g., missing tool definitions when tool calls are present). We then aggressively filter reasoning traces exhibiting pathological repetition, such as repeated n-grams within a sliding window or across the entire trajectory, which we found to be a strong indicator of malformed or low-quality reasoning. Finally, based on internal audits of synthetically generated datasets, we observed that some teacher models occasionally produce reasoning traces and final responses that implicitly align with specific political entities or promote nationalistic narratives. To mitiga

…(truncated — see the full README on HuggingFace)
