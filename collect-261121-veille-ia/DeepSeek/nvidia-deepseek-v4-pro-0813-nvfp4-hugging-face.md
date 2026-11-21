---
id: collect-261121-veille-ia/deepseek/nvidia-deepseek-v4-pro-0813-nvfp4-hugging-face
title: "nvidia/DeepSeek-V4-Pro-0813-NVFP4 · Hugging Face"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["model", "nvidia", "dataset", "deepseek-v4-pro-0813-nvfp4", "deepseek-v4-pro-0813", "attention", "output", "data"]
source: https://huggingface.co/nvidia/DeepSeek-V4-Pro-0813-NVFP4
source_anchor: ""
source_lines: [1, 202]
section: "DeepSeek"
---
The NVIDIA DeepSeek-V4-Pro-0813-NVFP4 model is the quantized version of DeepSeek AI's DeepSeek-V4-Pro-0813 model, an autoregressive Mixture-of-Experts language model that uses an optimized Transformer architecture with hybrid attention (Compressed Sparse Attention and Heavily Compressed Attention) and Manifold-Constrained Hyper-Connections. DeepSeek-V4-Pro-0813 is the official DeepSeek-V4-Pro release with enhanced agentic capabilities, and includes DeepSeek's DSpark speculative decoding module in the same checkpoint. For more information, refer to the DeepSeek-V4-Pro-0813 model card. The NVIDIA DeepSeek-V4-Pro-0813-NVFP4 model is quantized with Model Optimizer.

This model is ready for commercial or non-commercial use.  

This model is not owned or developed by NVIDIA. This model has been developed and built to a third-party’s requirements for this application and use case; see link to Non-NVIDIA (DeepSeek-V4-Pro-0813) Model Card.

Global 

DeepSeek V4 is well-suited for advanced reasoning, agentic AI applications, tool use scenarios, and complex problem-solving in domains such as mathematics, software engineering, and enterprise AI assistants.  

Hugging Face 08/27/2026 via https://huggingface.co/nvidia/DeepSeek-V4-Pro-0813-NVFP4 

Nvidia Model Optimizer: https://github.com/NVIDIA/Model-Optimizer

**Architecture Type:** Transformers  

**Network Architecture:** DeepseekV4ForCausalLM — Mixture-of-Experts (MoE) with Hybrid Attention (Compressed Sparse Attention + Heavily Compressed Attention) 

**Number of Model Parameters:** 1.65T in total and **49B** activated 

**This model was developed based on DeepSeek-V4-Pro-0813** 

**Input Type(s):** Text 

**Input Format(s):** String 

**Input Parameters:** One-Dimensional (1D): Sequences 

**Other Properties Related to Input:** Supports multi-turn conversations with system prompts, user messages, and assistant responses. Maximum context length of 1 million tokens. Uses a custom encoding pipeline (encoding_dsv4) with reasoning effort levels (`low`, `high`, `max`). 

**Output Type(s):** Text 

**Output Format:** String 

**Output Parameters:** One-Dimensional (1D): Sequences 

**Other Properties Related to Output:** Supports structured JSON output, function/tool calling, and reasoning content when enabled. 

Our AI models are designed and/or optimized to run on NVIDIA GPU-accelerated systems. By leveraging NVIDIA’s hardware (e.g. GPU cores) and software frameworks (e.g., CUDA libraries), the model achieves faster training and inference times compared to CPU-only solutions. 

**Supported Runtime Engine(s):** 

- **SGLang**
- **vLLM**

**Supported Hardware Microarchitecture Compatibility:** 

- NVIDIA Blackwell B200

**Preferred Operating System(s):** 

- Linux 

The integration of foundation and fine-tuned models into AI systems requires additional testing using use-case-specific data to ensure safe and effective deployment. Following the V-model methodology, iterative testing and validation at both unit and system levels are essential to mitigate risks, meet technical and functional requirements, and ensure compliance with safety and ethical standards before deployment.

The model is DeepSeek-V4-Pro-0813 NVFP4 quantized with nvidia-modelopt **v0.47.0rc1**  

**Link:** cnn_dailymail, Nemotron-Post-Training-Dataset-v2 

**Data Collection Method by dataset:** Automated. 

**Labeling Method by dataset:** Automated. 

**Properties:** The cnn_dailymail dataset is an English-language dataset containing just over 300k unique news articles as written by journalists at CNN and the Daily Mail. The Nemotron-Post-Training-Dataset-v2 is a post-training dataset curated by NVIDIA containing multi-turn conversations across diverse topics. 

**Data Modality:** Undisclosed 

**Data Collection Method by dataset:** Undisclosed 

**Labeling Method by dataset:** Undisclosed 

**Properties:** Undisclosed

**Datasets:** GPQA Diamond, AA-LCR, τ²-Bench Telecom, SciCode, IFBench, Terminal-Bench Hard 

**Data Collection Method by dataset:** Hybrid: Automated, Manually-Collected 

**Labeling Method by dataset:** Hybrid: Manually-Labeled, Automated 

**Properties:** We evaluated the model on reasoning, long-context recall, agentic tool-use, coding, and instruction-following benchmarks: GPQA Diamond contains 448 graduate-level multiple-choice questions written by domain experts in biology, physics, and chemistry. AA-LCR (Artificial Analysis Long Context Recall) evaluates a model's ability to accurately retrieve and recall information from long input contexts. τ²-Bench Telecom evaluates agentic tool-use and policy-adherence capabilities in dual-control telecom customer-service scenarios where the model interacts with a simulated user and external tools to resolve account issues. SciCode evaluates scientific coding capabilities. IFBench is a benchmark for evaluating instruction-following capabilities across diverse and structured task constraints. Terminal-Bench Hard evaluates agentic software-engineering capability in a real terminal environment, where the model must plan and execute shell commands to complete tasks end to end. 

**Acceleration Engine:** **SGLang** (benchmarks in this card) 

**Test Hardware:** **NVIDIA Blackwell B200** 

This model was obtained by quantizing the weights and activations of DeepSeek-V4-Pro-0813 to NVFP4 data type, ready for inference with **SGLang** and **vLLM**. Only the weights and activations of the linear operators within transformer blocks in MoE are quantized. DeepSeek's DSpark speculative-decoding heads are carried through unquantized and unmodified.

The recipe is in the Model Optimizer repository under
`examples/deepseek/deepseek_v4`
(`ptq.py`,
`quantize_to_nvfp4.py`).
It runs in three stages:

**1. Reshard the source checkpoint to the model-parallel layout** (CPU only), using DeepSeek's own `convert.py`:

```
python convert.py \
    --hf-ckpt-path  <DeepSeek-V4-Pro-0813> \
    --save-path     <mp8_checkpoint> \
    --n-experts     384 \
    --model-parallel 8 \
    --expert-dtype  fp4
```
**2. Collect activation ranges (amax) on the routed experts:**

```
torchrun --nproc-per-node 8 deepseek_v4/ptq.py \
    --model_path  <mp8_checkpoint> \
    --config      <DeepSeek-V4-Pro-0813>/inference/config.json \
    --calib_size  512 \
    --calib_seq   4096 \
    --output_path <amax_dump>
```
Calibration uses the script's default datasets — `cnn_dailymail` and
`Nemotron-Post-Training-Dataset-v2` — at 512 samples each (1024 total).
`--calib_seq 4096` sets the tokenizer truncation cap; it is the setting that
mattered most for this model, since the default of 512 does not cover
long-context activation ranges.

The command above uses `ptq.py`'s built-in quantization config. The same config also ships
as a declarative recipe, selectable with `--recipe`:

```
torchrun --nproc-per-node 8 deepseek_v4/ptq.py \
    --recipe models/deepseek-ai/DeepSeek-V4-Pro-0813/ptq/nvfp4_experts_only \
    ...
```
The recipe YAML is pinned by a test to match the built-in default, so either path yields the same quantizer configuration.

**3. Export the NVFP4 checkpoint:**

```
python deepseek_v4/quantize_to_nvfp4.py \
    --amax_path   <amax_dump> \
    --source_ckpt <DeepSeek-V4-Pro-0813> \
    --output_ckpt <output> \
    --cast_mxfp4_to_nvfp4 \
    --device cuda
```
`--cast_mxfp4_to_nvfp4` matters for this model in particular. DeepSeek-V4 already
ships its routed experts in **MXFP4** (E2M1 nibbles with one E8M0 scale per 32
elements). NVFP4 reuses the same E2M1 nibble grid but stores an E4M3 scale per 16
elements, so the weight bits are carried over unchanged — a lossless bit-cast —
and only the block scales are rewritten. The activation amax from stage 2 is
therefore the only calibrated quantity that survives into the artifact, and the
checkpoint is slightly *larger* than its source (scale bytes double while weight
bytes stay the same).

```
python3 -m sglang.launch_server \
    --model-path nvidia/DeepSeek-V4-Pro-0813-NVFP4 \
    --tp 8 \
    --trust-remote-code \
    --tool-call-parser deepseekv4 \
    --reasoning-parser deepseek-v4
```
The base DeepSeek-V4-Pro-0813 checkpoint includes DSpark and those heads are preserved here, but speculative decoding was not exercised during this release's validation — enable it only after verifying it for this NVFP4 checkpoint.

```
vllm serve nvidia/DeepSeek-V4-Pro-0813-NVFP4 \
    --served-model-name DeepSeek-V4-Pro-0813-NVFP4 \
    --tensor-parallel-size 8 \
    --enable-expert-parallel \
    --reasoning-parser deepseek_v4 \
    --max-model-len 400000 \
    --gpu-memory-utilization 0.9 \
    --kv-cache-dtype fp8 \
    --max-num-batched-tokens 8192 \
    --enable-chunked-prefill \
    --model-loader-extra-config '{"enable_multithread_load": true, "num_threads": 128}'
```
vLLM selects the NVFP4 MoE backend automatically; no backend environment variables
are required. `--model-loader-extra-config` parallelises the checkpoint load, which
otherwise dominates startup for a checkpoint this size. Tune `--max-num-seqs` to your
own concurrency — it is a throughput knob and does not affect output.

This configuration was validated on 8xB200 (TP=8) against this checkpoint with both
`vllm/vllm-openai:v0.27.1` and `v0.28.0`, running a full long-context evaluation
(~115K-token prompts, ~128K total) with no errors on either. It was contributed and run
by NVIDIA ModelOpt QA rather than measured for the table below, which was produced with
SGLang.

The accuracy benchmark results are presented in the table below:

| **Precision** | **GPQA Diamond** | **AA-LCR** | **τ²-Bench Telecom** | **SciCode** | **IFBench** | **Terminal-Bench Hard** | 
| MXFP4 (source) | **88.51** | **68.67** | **96.49** | **53.45** | **76.53** | **51.39** | 
| NVFP4 | **88.42** | **69.33** | **98.25** | **53.75** | **75.68** | **50.69** | 

Baseline: DeepSeek-V4-Pro-0813, whose routed experts already ship in MXFP4. Both checkpoints are mixed precision and share the same FP8 attention and shared-expert projections, BF16 norms and embeddings; only the routed-expert format differs (MXFP4 → NVFP4).
All scores measured on SGLang at `temperature 1.0`, `top_p 1.0`, max reasoning effort. GPQA Diamond, SciCode, τ²-Bench Telecom and Terminal-Bench Hard are single-run; IFBench is pooled over 4 runs per side; AA-LCR is the matched-configuration pair (2 runs per side).


The base model was trained on data that contains toxic language and societal biases originally crawled from the internet. Therefore, the model may amplify those biases and return toxic responses especially when prompted with toxic prompts. The model may generate answers that may be inaccurate, omit key information, or include irrelevant or redundant text producing socially unacceptable or undesirable text, even if the prompt itself does not include anything explicitly offensive.

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. Developers should work with their internal model team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse.

Users are responsible for model inputs and outputs. Users are responsible for ensuring safe integration of this model, including implementing guardrails as well as other safety mechanisms, prior to deployment.

Please report model quality, risk, security vulnerabilities or NVIDIA AI Concerns here.

- Downloads last month
- 580
