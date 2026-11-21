---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-super-vl-120b-moe-multimodal-architecture-600-tps-agentic-inference-and-enterpri
title: "NVIDIA Nemotron 3.5 Super VL: 120B MoE Multimodal Architecture, 600+ TPS Agentic Inference, and Enterprise Customization | Naren Vadapalli"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "nemotron", "super", "multimodal", "enterprise", "architecture", "inference", "model"]
source: https://www.narenvadapalli.com/blog/nvidia-nemotron-3-5-super-vl-specialized-multimodal-agents/
source_anchor: ""
source_lines: [1, 205]
section: "NVIDIA_Hardware"
---
# NVIDIA Nemotron 3.5 Super VL: 120B MoE Multimodal Architecture, 600+ TPS Agentic Inference, and Enterprise Customization

Inside NVIDIA Nemotron 3.5 Super VL: 120B Mixture-of-Experts with 12B active parameters, 600+ tokens/sec agentic throughput, LiLiCorr speculative decoding, and NeMo post-training recipes.

*Series: â Inside Meta Muse Wonder & Muse Wonder Full: Frontier Models for Personal and Professional Agents (Previous) | Gemini 4 Argon for Robotics: Physical AI Reasoning, Real2Sim MuJoCo Twins, and Closed-Loop Control (Next) â*

### Prior Reading Material

- vLLM on NVIDIA Vera Rubin: 7.8x Throughput, HBM4 Memory Hierarchy, and the NVL72 Inference Leap â NVIDIAâs next-generation silicon substrate, NVFP4 Tensor Core quantization, and memory bandwidth hierarchies for MoE serving.
- vLLM Decision 3.0 (d3): The Open Multimodal System One Decision Model Family â Reflex-speed multimodal decision models for visual routing and sub-second tool dispatch.
- What is an Omni Model? Architectural Taxonomy Across Qwen, Gemini, and NVIDIA Cosmos â Cross-modal fusion mechanics, continuous vision-language embeddings, and unified tokenization across enterprise agent loops.

### Summary

On October 15, 2026, NVIDIA unveiled **Nemotron 3.5 Super VL**, an open-weight, highly customizable multimodal foundation model engineered specifically for always-on enterprise agent swarms. While frontier proprietary models excel at open-ended creative dialogue, production autonomous agents require a vastly different operational profile: high-speed multi-step tool execution, simultaneous document and chart comprehension, strict policy adherence, and sub-second response latenciesâall without bankrupting infrastructure budgets.

Nemotron 3.5 Super VL tackles this bottleneck through a **120-billion-parameter Mixture-of-Experts (MoE)** architecture that routes each token through only **12 billion active parameters**. By combining an interleaved Mamba-2 state-space backbone with attention layers, a **C-RADIO** vision encoder, native Multi-Token Prediction (MTP), and **LiLiCorr speculative decoding**, the model shatters previous throughput boundaries. On the Artificial Analysis Intelligence Index benchmark deployed on Baseten endpoints, Nemotron 3.5 Super VL clocks an astonishing **600+ output tokens per second (TPS)**âoutperforming comparable open vision-language models by more than **5x** while running seamlessly on local desktop supercomputers like the NVIDIA DGX Spark and enterprise Blackwell clusters.

### Hugging Face & Official Model Card Summary

| Specification | Technical Detail | 
|---|---|
| **Model Repository** | `nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-Base-BF16` | 
| **Quantized NVFP4 Checkpoint** | `nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4` | 
| **Speculative Drafter Checkpoint** | `nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4-LiLiCorr` | 
| **Total Parameters** | 120 Billion | 
| **Active Parameters / Token** | 12 Billion (Top-routed MoE experts) | 
| **Architecture** | Hybrid Interleaved Mamba-2 / Transformer MoE + C-RADIO Vision Encoder | 
| **Native Context Length** | Up to 1,048,576 tokens (1M Context) | 
| **Supported Modalities** | Text, Code, High-Resolution Images, Multipage PDF Documents, Video Streams | 
| **Inference Frameworks** | NVIDIA NIM, vLLM, SGLang, TensorRT-LLM | 
| **License** | OpenMDW License Agreement v1.1 (Open Commercial & Research Use) | 
| **Release Announcement** | NVIDIA Developer Technical Blog | 
| **Technical Report** | NVIDIA Research Nemotron 3.5 Technical Report | 

### The Story: The High-Throughput Multimodal Agent Dilemma

Imagine an enterprise automation loop tasked with processing corporate credit card reconciliations. A receipt arrives as a blurred mobile snapshot taken under fluorescent lights. The receipt contains handwritten server tips, folded tax tables, and faded vendor logos.

In traditional agent workflows, this single task requires a cumbersome multi-model pipeline:

```
flowchart TD
    A["Raw Document / Receipt Image"] --> B["External Vision API / OCR Parser"]
    B --> C["Extracted Text Payload & Bounding Boxes"]
    C --> D["Context Window Serializer"]
    D --> E["Frontier LLM Reasoning Node"]
    E --> F["ERP Tool Call Formulation"]
    F --> G["Database Execution & Ledger Update"]
```
This multi-model chain breaks down in production due to three distinct failure modes:

1. **Context Serializing Overhead** : Passing raw bounding boxes, layout coordinates, and OCR snippets back and forth between separate vision and text models balloons the prompt size and introduces latency compounding.
2. **Visual Semantic Disconnect** : A pure text LLM cannot cross-reference a layout cue (e.g., an asterisk pointing to fine print at the bottom of a page) with the tabular line items above it.
3. **The 30-Second Freeze** : When an agent must parse 40 pages of audited filings, waiting 35 seconds per page for a cloud-hosted frontier model grinds the enterprise pipeline to a halt.

Nemotron 3.5 Super VL collapses this disparate pipeline into a single, cohesive neural substrate:

```
flowchart TD
    A["Raw Document / Video / Multi-Image Input"] --> B["Nemotron 3.5 Super VL Unified Model"]
    B --> C["Simultaneous Visual Layout Perception & Token Routing"]
    C --> D["Direct Structured Tool Emission & Action Execution"]
    D --> E["Single-Pass Enterprise Action at 600+ TPS"]
```
### Intelligence Index vs. Output Speed: The 5x Baseten Leap

For autonomous agents that invoke tools repeatedly in iterative search loops, **output speed (Tokens Per Second - TPS)** is not a cosmetic metricâit directly determines whether an agent swarm finishes a multi-step debugging task in 10 seconds or 10 minutes.

On the Artificial Analysis Intelligence Index benchmark evaluating agentic tasks over long context (100,000 input tokens), Nemotron 3.5 Super VL achieves an unprecedented position on the speed-intelligence frontier:

*Figure 1. Nemotron 3.5 Super VL delivers over 600 TPS on Artificial Analysis Intelligence Index agentic tasks on the Baseten endpoint. Source: NVIDIA Technical Blog / Artificial Analysis.*

As shown in Figure 1, competing open-weight models such as Inkling Small (122 TPS), Ling 3.0 Flash (330 TPS), and MiniMax-M3 (115 TPS) cluster on the left side of the speed spectrum. Nemotron 3.5 Super VL rockets into the coveted **Most Attractive Quadrant**, generating **over 600 output tokens per second**âa **5x speedup** over Inkling Small while maintaining competitive Intelligence Index scores.

```
flowchart TD
    A["High Intelligence (> 40 Index) / Low Speed (< 150 TPS)"] --> B["Traditional Dense Frontier APIs"]
    B --> C["Bottleneck: Multi-Turn Agent Stalls"]
    C --> D["Nemotron 3.5 Super VL Sweet Spot"]
    D --> E["27+ Intelligence Index at 625+ TPS"]
    E --> F["Immediate Interactive Agency & Real-Time Tool Looping"]
```
### Local Workstation Execution: Pareto Frontier on NVIDIA DGX Spark

Deploying frontier multimodal models in data-sensitive enterprise environments often requires local, on-premise execution to satisfy data sovereignty, compliance, and zero-data-retention mandates.

Independent evaluations conducted by **EXO Labs** evaluated Nemotron 3.5 Super VL against contemporary open and proprietary models running on the NVIDIA DGX Spark (NVIDIAâs desktop AI supercomputer equipped with Grace Blackwell architecture and 128 GB of unified memory):

*Figure 4. Nemotron 3.5 Super VL balances accuracy and speed for agent tasks on DGX Spark. Source: NVIDIA Developer Technical Blog / EXO Labs Independent Evaluation.*

By leveraging **NVFP4 (4-bit floating point) quantization** and **LiLiCorr speculative decoding**, Nemotron 3.5 Super VL achieves a dominant position on the Pareto frontier:

- **Accuracy Parity** : Matches within 1.5% of full-precision BF16 baselines (78.4% vs 78.9% on agentic task completion).
- **Throughput Leap on DGX Spark** : Delivers**168 TPS locally** âmore than 3.6x faster than Qwen 2.5 VL 72B (46 TPS) and 6x faster than Llama 3.2 90B Vision (28 TPS).
- **Memory Footprint** : Fits cleanly within the unified 128 GB memory boundary of a single DGX Spark desktop workstation, enabling local enterprise agent execution without requiring multi-node datacenter racks.

### Architectural Anatomy: Hybrid Mamba-2 MoE & C-RADIO Vision

Nemotron 3.5 Super VLâs efficiency stems from a deeply integrated four-pillar architectural stack:

```
flowchart TD
    A["Multimodal Inputs (Images, PDF, Video, Text)"] --> B["C-RADIO Vision Encoder"]
    B --> C["Dynamic High-Resolution Visual Projector"]
    C --> D["Interleaved Mamba-2 State-Space Layers"]
    D --> E["Top-K Mixture-of-Experts Router"]
    E --> F["512 Routed Expert Blocks (12B Active Tokens)"]
    F --> G["Native Multi-Token Prediction (MTP) Head"]
    G --> H["Speculative Decoding Verification (LiLiCorr Drafter)"]
```
#### 1. C-RADIO Multimodal Backbone

Instead of relying on standard CLIP encoders that lose fine typographic resolution, Nemotron 3.5 Super VL integrates **C-RADIO** (Continuous Robust Adaptive Visual Representation). C-RADIO dynamically tiles ultra-high-resolution documents into variable-aspect patches, enabling the model to extract sub-pixel table dividers, fine financial footers, and microscopic code indentation.

#### 2. Hybrid Mamba-2 / Attention Layers

Long-horizon agent execution quickly accumulates hundreds of thousands of context tokens as tool call logs grow. By interleaving linear-time Mamba-2 state-space operators with standard self-attention layers, the model prevents the quadratic $O(N^2)$ memory wall during long-context prefill while maintaining the sharp associative recall of multi-head attention.

#### 3. 120B / 12B Active Sparse MoE

The model splits feed-forward processing across 512 fine-grained routed experts. For any single token, the routing gate activates only 12B parameters. This architecture slashes compute requirements by 90% compared to equivalent dense 120B architectures, allowing streaming multiprocessors (SMs) to sustain high clock frequencies and maximize Tensor Core saturation.

#### 4. Native Speculative Decoding (LiLiCorr & MTP)

During inference, a companion lightweight drafter model (**LiLiCorr**) speculative-proposes candidate token sequences. Nemotron 3.5 Super VLâs native Multi-Token Prediction (MTP) head verifies and accepts 3 to 4 tokens in parallel per forward pass, multiplying effective decoding throughput without degrading output perplexity.

### Enterprise Customization: Post-Training with NVIDIA NeMo

Off-the-shelf foundation models rarely understand internal ERP schemas, proprietary ticketing taxonomy, or complex regulatory guardrails. Nemotron 3.5 Super VL is designed from the ground up for post-training adaptation using NVIDIA NeMo.

*Figure 2. Post-trained Nemotron 3.5 Super VL achieves leading performance for specialized tasks across industries. Source: NVIDIA Developer Technical Blog.*

Enterprises across sectors have demonstrated massive operational gains through domain post-training:

1. **Scale AI & Skydance** : Post-trained on automated vendor credit memo processing across complex entertainment production documents, cutting**inference cost by 40x** and**latency by 5x** .
2. **Ramp** : Post-trained for autonomous receipt parsing, converting skewed receipts into structured financial JSON with**50% higher throughput** and**10x lower latency** .
3. **AI21 & Alice** : Post-trained on threat intelligence feeds and Common Vulnerabilities and Exposures (CVE) logs, boosting cybersecurity triage**accuracy by 41%** .

The NeMo post-training recipe follows a two-stage alignment cycle:

```
flowchart TD
    A["Raw Domain Corpora & Unlabeled Receipts/Logs"] --> B["NeMo Data Designer: Synthetic Q&A Generation"]
    B --> C["Stage 1: Supervised Fine-Tuning (SFT / LoRA)"]
    C --> D["NVIDIA NeMo Gym: Environment Trajectories"]
    D --> E["Stage 2: Reinforcement Learning with Verifiable Rewards (RLVR)"]
    E --> F["Domain-Specialized Nemotron 3.5 Super VL Agent"]
```
### Running Nemotron 3.5 Super VL Locally: Python & vLLM Implementation

Deploying Nemotron 3.5 Super VL with 4-bit NVFP4 quantization and speculative decoding in production is straightforward using modern inference engines like vLLM.

## **Click to expand runnable code script**

```
"""
nemotron_agent_serving.py
Serving NVIDIA Nemotron 3.5 Super VL with NVFP4 Quantization and Tool Calling via vLLM.
"""
import os
from vllm import LLM, SamplingParams
from PIL import Image
def init_nemotron_engine():
    # Initialize the 120B/12B MoE model with NVFP4 precision
    llm = LLM(
        model="nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4",
        speculative_model="nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4-LiLiCorr",
        num_speculative_tokens=4,
        tensor_parallel_size=1,  # Single GPU on DGX Spark or B200
        quantization="nvfp4",
        max_model_len=65536,
        trust_remote_code=True
    )
    return llm
def analyze_document_and_act(llm: LLM, image_path: str, prompt: str):
    image = Image.open(image_path).convert("RGB")
    
    # Formulate multimodal agent payload
    payload = {
        "prompt": f"<image>\nTask: {prompt}\nRespond with structured JSON tool calls.",
        "multi_modal_data": {"image": image}
    }
    
    sampling_params = SamplingParams(
        temperature=0.1,
        max_tokens=1024,
        stop=["<|eot_id|>"]
    )
    
    outputs = llm.generate([payload], sampling_params=sampling_params)
    for output in outputs:
        print("Generated Agent Action Plan:")
        print(output.outputs[0].text)
if __name__ == "__main__":
    print("Initializing NVIDIA Nemotron 3.5 Super VL Serving Engine...")
    # Example execution (requires GPU with NVFP4 support, e.g., Blackwell/Spark):
    # engine = init_nemotron_engine()
    # analyze_document_and_act(engine, "credit_memo.png", "Extract invoice items and approve credit limit.")
```
### Conclusion & Whatâs Next

The release of **Nemotron 3.5 Super VL** signals a fundamental shift in enterprise agent architectures. By breaking free from the trade-off between visual reasoning fidelity and execution latency, NVIDIA has delivered an open, customizable engine capable of running both in hyperscale clouds at **600+ TPS** and locally on desktop workstations at **168 TPS**.

For engineering teams building autonomous workflows, Nemotron 3.5 Super VL offers the holy grail: the parameter capacity of a 120B model, the inference efficiency of a 12B model, and the complete sovereignty of open-weight customization.
