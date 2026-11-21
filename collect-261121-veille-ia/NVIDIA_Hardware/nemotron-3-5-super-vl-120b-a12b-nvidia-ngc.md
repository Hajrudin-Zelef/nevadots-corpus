---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-3-5-super-vl-120b-a12b-nvidia-ngc
title: "Nemotron-3.5-Super-VL-120B-A12B | NVIDIA NGC"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "model", "video", "super-vl-120b-a12b", "image", "vision-language", "input", "nvidia-nemotron-3"]
source: https://catalog.ngc.nvidia.com/orgs/nim/nvidia/containers/nemotron-3.5-super-vl-120b-a12b/latest
source_anchor: ""
source_lines: [1, 67]
section: "NVIDIA_Hardware"
---
NVIDIA Nemotron 3.5 Super VL 120B-A12B NIM: vision-language model for image, video, and text input, served with vLLM on Blackwell, Hopper, and Ampere GPUs.

# **NVIDIA-Nemotron-3.5-Super-VL-120B-A12B Overview**

## **Description:**

This container houses **NVIDIA-Nemotron-3.5-Super-VL-120B-A12B**, a vision-language model (VLM) trained by NVIDIA that accepts image, video, and text input and produces text output. It is designed for reasoning, coding, tool use, document understanding, and video understanding. The model first generates a reasoning trace and then a final response; reasoning can be turned on or off through the chat template (`enable_thinking`).

The model combines interleaved Mamba-2, Mixture-of-Experts (MoE), and attention layers with a C-RADIO vision encoder and a vision-language connector. It has **120B total parameters** and **12B active parameters** and supports context lengths up to **1M tokens**. Multi-Token Prediction (MTP) enables native speculative decoding; the NIM uses it on every profile.

The supported languages include: English, French, German, Italian, Japanese, Spanish, and Chinese.

The container components are ready for commercial/non-commercial use.

### **License/Terms of Use:**

**GOVERNING TERMS:** The NIM container is governed by the NVIDIA Software License Agreement and the Product-Specific Terms for NVIDIA AI Products.

You are responsible for ensuring that your use of any provided models complies with all applicable laws.

### **Deployment Geography:**

Global

### **Release Date:**

Hugging Face 10/15/2026 via Hugging Face BF16 and Hugging Face NVFP4 

## Program Classes

The NVIDIA-Nemotron-3.5-Super-VL-120B-A12B container includes the following model:

| Model Name & Link | Use Case | How to Pull the Model | 
|---|---|---|
| NVIDIA-Nemotron-3.5-Super-VL-120B-A12B | Vision-language model for developers deploying agents for coding, search, tool-assisted workflows, document question answering, and image and video understanding. BF16 and NVFP4 weights; the NIM downloads the version that matches the selected profile. | Automatic | 

## Deployment Details:

Our AI models are designed and/or optimized to run on NVIDIA GPU-accelerated systems. By leveraging NVIDIA's hardware (e.g. GPU cores) and software frameworks (e.g., CUDA libraries), the model achieves faster training and inference times compared to CPU-only solutions.

**Supported GPUs:** NVIDIA Blackwell (B200, GB200, B300, GB300, RTX Pro 6000 Blackwell Server Edition, DGX Station, DGX Spark); NVIDIA Hopper (H100, H200); NVIDIA Ampere (A100 80GB). Pin a profile with `NIM_MODEL_PROFILE`; list the available profiles with `list-model-profiles`. Video input needs `NVIDIA_DRIVER_CAPABILITIES=compute,utility,video` so the NIM can decode on the GPU (NVDEC).

**Documentation**

Visit the NIM for Vision Language Models page for release documentation, deployment guides, and more.

## Security Common Vulnerabilities and Exposures (CVEs)

Please review the Security Scanning tab on NGC to view the latest security scan results. For certain open-source vulnerabilities listed in the scan results, NVIDIA provides a response in the form of a Vulnerability Exploitability eXchange (VEX) document. The VEX information can be reviewed and downloaded from the Security Scanning tab.

## **Ethical Considerations:**

NVIDIA believes Trustworthy AI is a shared responsibility and we have established policies and practices to enable development for a wide array of AI applications. When downloaded or used in accordance with our terms of service, developers should work with their internal teams to ensure these software components meet requirements for the relevant industry and use case and addresses unforeseen product misuse.

Please make sure you have proper rights and permissions for all input image and video content; if image or video includes people, personal health information, or intellectual property, the model output will not blur or maintain proportions of image subjects included.

Please report quality, risk, security vulnerabilities or NVIDIA AI Concerns here.

## **Get Help**

### NVIDIA Developer Community Forum

Get access to community knowledge base articles and support cases (https://forums.developer.nvidia.com/)

### Enterprise Support

Get access to knowledge base articles and support cases or submit a ticket.
