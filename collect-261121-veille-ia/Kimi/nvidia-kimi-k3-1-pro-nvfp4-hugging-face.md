---
id: collect-261121-veille-ia/kimi/nvidia-kimi-k3-1-pro-nvfp4-hugging-face
title: "nvidia/Kimi-K3.1-Pro-NVFP4 · Hugging Face"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "nvidia", "kimi-k3", "dataset", "method", "data", "parameters", "properties"]
source: https://huggingface.co/nvidia/Kimi-K3.1-Pro-NVFP4
source_anchor: ""
source_lines: [1, 132]
section: "Kimi"
---
The NVIDIA Kimi-K3.1-Pro NVFP4 model is the quantized version of Moonshot AI's Kimi-K3.1-Pro model, which is an auto-regressive language model that uses an optimized transformer architecture. Kimi-K3.1-Pro is a natively multimodal Mixture-of-Experts model post-trained from Kimi K3, with 2.8T total parameters, 104B activated parameters, and a 1,048,576-token context window. For more information, please check here. The NVIDIA Kimi-K3.1-Pro NVFP4 model is quantized with Model Optimizer.

This model is ready for commercial or non-commercial use. 

This model is not owned or developed by NVIDIA. This model has been developed and built to a third-party's requirements for this application and use case; see link to Non-NVIDIA (Kimi-K3.1-Pro) Model Card from Moonshot AI.

**GOVERNING DOWNLOAD TERMS:** Use of this model is governed by the NVIDIA Open Model Agreement.

**ADDITIONAL INFORMATION** : The base model Kimi-K3.1-Pro is governed by the Kimi-K3.1-Pro License. **Kimi-K3.1-Pro**.

Global 

Developers looking to take off-the-shelf, pre-quantized models for deployment in AI Agent systems, chatbots, RAG systems, and other AI-powered applications. 

Huggingface **11/10/2026** via https://huggingface.co/nvidia/Kimi-K3.1-Pro-NVFP4 

NVIDIA Model Optimizer: https://github.com/NVIDIA/Model-Optimizer

**Architecture Type:** Transformer 

**Network Architecture:**  Multimodal Mixture-of-Experts (MoE) 

**Number of Model Parameters:** 2.8T in total and 104B activated 

**This model was developed based on Kimi-K3.1-Pro.** 

**Input Type(s):** Text, Image 

**Input Format(s):** String, Red, Green, Blue (RGB) 

**Input Parameters:** Text: One-Dimensional (1D); Image: Two-Dimensional (2D) 

**Other Properties Related to Input:** Context length up to 1,048,576 tokens. Images are encoded by MoonViT-V2. 

**Output Type(s):** Text 

**Output Format:** String 

**Output Parameters:** 1D (One-Dimensional): Sequences 

**Other Properties Related to Output:** None 

Our AI models are designed and/or optimized to run on NVIDIA GPU-accelerated systems. By leveraging NVIDIA's hardware (e.g. GPU cores) and software frameworks (e.g., CUDA libraries), the model achieves faster training and inference times compared to CPU-only solutions. 

**Runtime Engine(s):** 

- **vLLM**
- **SGLang**

**Supported Hardware Microarchitecture Compatibility:** 

- NVIDIA Blackwell 

**Supported Operating System(s):** 

- Linux 

The integration of foundation and fine-tuned models into AI systems requires additional testing using use-case-specific data to ensure safe and effective deployment. Following the V-model methodology, iterative testing and validation at both unit and system levels are essential to mitigate risks, meet technical and functional requirements, and ensure compliance with safety and ethical standards before deployment.

The model version is NVFP4 1.0 version and is quantized with nvidia-modelopt **v0.48.0** 

**Link:** cnn_dailymail, Nemotron-Post-Training-Dataset-v2 

**Data Collection Method by dataset:** Automated. 

**Labeling Method by dataset:** Automated. 

**Properties:** The cnn_dailymail dataset is an English-language dataset containing just over 300k unique news articles as written by journalists at CNN and the Daily Mail. The Nemotron-Post-Training-Dataset-v2 is a post-training dataset curated by NVIDIA containing multi-turn conversations across diverse topics. 

**Data Modality:** Undisclosed 

**Data Collection Method by dataset:** Undisclosed 

**Labeling Method by dataset:** Undisclosed 

**Properties:** Undisclosed

**Data Collection Method by dataset:** Not Applicable 

**Labeling Method by dataset:** Not Applicable 

**Properties:** Not Applicable. NVIDIA did not perform a separate testing-dataset pass for this quantized checkpoint. 

**Datasets:** GPQA Diamond, SciCode, Tau2-bench Telecom, MMMU-Pro, AA-LCR, and Terminal-Bench 2.1 

**Data Collection Method by dataset:** Hybrid: Automated, Manually-Collected 

**Labeling Method by dataset:** Hybrid: Manually-Labeled, Automated 

**Properties:** We evaluated the model on text-based reasoning and multimodal benchmarks: GPQA Diamond contains 448 graduate-level multiple-choice questions written by domain experts in biology, physics, and chemistry;  MMMU Pro is the more challenging version of the Massive Multi-discipline Multimodal Understanding benchmark, measuring college-level multimodal reasoning across diverse disciplines with expanded answer choices and a vision-only input setting. 

**Acceleration Engine:** **vLLM**, **SGLang** 

**Test Hardware:** **NVIDIA Blackwell GB300** 

This model was obtained by quantizing the weights and activations of Kimi-K3.1-Pro to NVFP4 data type, ready for inference with **vLLM** and **SGLang**. The source checkpoint stores MXFP4 weights. The routed MoE expert weights are converted from MXFP4 to NVFP4 and the input_scales are set to 1.0. The shared experts and the KDA attention are quantized to MXFP8 and the activation scales are learnt on the fly. The MLA layers are kept in BF16 for the best accuracy recovery.

- **vLLM** : To serve this checkpoint with vLLM, you can start the docker`vllm/vllm-openai:kimi-k31-cu134` and run the sample command below:

```
vllm serve nvidia/Kimi-K3.1-Pro-NVFP4 --port 8000 --tensor-parallel-size 8 --max-model-len 1048576 --max-num-seqs 256 --load-format fastsafetensors --trust-remote-code --reasoning-parser kimi_k3 --enable-auto-tool-choice --tool-call-parser kimi_k3
```
- **SGLang** : To serve this checkpoint with SGLang, you can start the latest nightly docker`lmsysorg/sglang:dev-cu13` and run the sample command below:

```
sglang serve --model-path nvidia/Kimi-K3.1-Pro-NVFP4 --tp 8 --trust-remote-code --max-running-requests 256 --reasoning-parser kimi_k3 --tool-call-parser kimi_k3
```
| **Precision** | **GPQA Diamond** | **AA-LCR** | **IFBench** | **MMMU Pro** | **SciCode** | **TB2.1** | 
| MXFP4 | **93.97** | **81.69** | **73.40** | **81.68** | **59.52** | **88.20** | 
| NVFP4 | **93.84** | **82.06** | **73.40** | **81.79** | **59.47** | **87.78** | 

Baseline: Kimi-K3.1-Pro, evaluated with Temperature 1.0, top-p 0.95


We also evaluate our NVFP4 checkpoint on the Kimi Vendor Verifier (KVV) which verifies whether a vendor deployment meets Kimi model accuracy and API feature requirements. We use the same settings as provided in the repository by KVV for the thinking mode, repeats, temperature, harness, etc.

| **Provdier** | **OCRBench** | **MMMU-Pro** | **ToolCall** | **BEAM (1M)** | **DeepSWE** | 
| Moonshot | **87.20** | **84.10** | **97.20** | **39.20** | **75.50** | 
| Nvidia NVFP4 | **86.70** | **84.10** | **97.40** | **38.70** | **77.40** | 

Both the accuracy benchmarks and the KVV evaluations use the main KV/ indexer cache in fp8, and the SSM state is in fp32.

The base model was trained on data that contains toxic language and societal biases originally crawled from the internet. Therefore, the model may amplify those biases and return toxic responses especially when prompted with toxic prompts. The model may generate answers that may be inaccurate, omit key information, or include irrelevant or redundant text producing socially unacceptable or undesirable text, even if the prompt itself does not include anything explicitly offensive.

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. Developers should work with their internal model team to ensure this model meets requirements for the relevant industry and use case and addresses unforeseen product misuse.

Please make sure you have proper rights and permissions for all input image and video content; if image or video includes people, personal health information, or intellectual property, the image or video generated will not blur or maintain proportions of image subjects included.

Please report model quality, risk, security vulnerabilities or NVIDIA AI Concerns here.

- Downloads last month
- 2,334
