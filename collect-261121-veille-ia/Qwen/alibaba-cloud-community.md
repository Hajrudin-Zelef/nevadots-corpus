---
id: collect-261121-veille-ia/qwen/alibaba-cloud-community
title: "Alibaba Cloud Community"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["qwen3", "attention", "model", "flash-next", "architecture", "context", "gated", "training"]
source: https://www.alibabacloud.com/blog/qwen3-8-flash-next-a-new-architecture-towards-ultimate-cost-efficiency_603501
source_anchor: ""
source_lines: [1, 374]
section: "Qwen"
---
# Qwen3.8-Flash-Next: A New Architecture, Towards Ultimate Cost-Efficiency

## Introduction

In this release we are opening the weights of **Qwen3.8-Flash-Next**, a multimodal MoE model that also serves as an early preview of the architecture used in **Qwen4**. It plays the same role that Qwen3-Next played for Qwen3.5: the hybrid **Gated DeltaNet + Gated Attention** design introduced at that time has since been used across the Qwen3.5, Qwen3.6, Qwen3.7 and Qwen3.8 series. We are again releasing the architectural changes early, so that the community can examine them before the full Qwen4 model family is built on top of them.

Qwen3.8-Flash-Next upgrades the model systematically along four aspects — **attention, residual, embedding and optimization** — improving model capability while further optimizing computational efficiency, model capacity and training stability:

- 
**Attention** : A**GDN + QSA hybrid architecture** . Gated DeltaNet (GDN) compresses the history efficiently;**Qwen Sparse Attention (QSA)** uses a compressed lightweight indexer to select the important context at micro-block granularity, substantially reducing the cost of attention on long sequences.
- 
**Residual** :**Gated Residual (GR)** widens the residual stream into 4 branches and controls reads and writes with a dynamic gate, strengthening cross-layer information flow and training stability.
- 
**Embedding** :**N-gram Embedding** looks up a table using the local context to scale model capacity with very little extra computation; the embedding table can be offloaded to host memory and overlapped with model computation through asynchronous prefetching.
- 
**Optimization** : The**Muon optimizer** is used, refined around orthogonalization accuracy, the division of labour between Muon and AdamW, and the splitting of fused parameters, with the scaling law refitted for the new architecture.

Qwen3.8-Flash-Next features a **125B**-parameter main model, supplemented by an additional **51B** N-gram embeddings, with **6B** parameters activated per token. Compared with Qwen3.7-Plus, Qwen3.8-Flash-Next substantially reduces both training and inference cost — training takes only about 1/9 as much, yet it delivers superior capabilities in coding and office tasks.

It natively supports **262,144** tokens of context and is extensible to **1,000,000** tokens with YaRN. For more technical details on the architecture, training methodology, and experimental analysis of Qwen3.8-Flash-Next, please refer to the technical report in our GitHub repository.

Qwen3.8-Flash-Next weights are now available on Hugging Face and ModelScope. The production version, with 1M context by default and official built-in tools, is served as **Qwen3.8-Flash** on QwenCloud, **priced at 0.16 USD per million input tokens and 0.47 USD per million output tokens**.

## Performance

### Language

### Vision Language

## Model Architecture

### Attention: GDN + QSA for Efficient Memory and Precise Retrieval

Traditional Full Attention provides direct access to all previous tokens, but as the context grows longer, both computation and KV Cache memory-access costs increase substantially.

Following the architecture design introduced in Qwen3.5, Qwen3.8-Flash-Next adopts a **GDN** <sup>1</sup>**+ Attention Hybrid architecture**: three out of every four layers use Gated DeltaNet (GDN) to continuously compress historical information into a fixed-size state, while the remaining layer uses global Attention for precise retrieval of information across the full context.

For global Attention, we further introduce **Qwen Sparse Attention (QSA)**. Sparse Attention reduces long-sequence computation by attending only to important context. However, existing approaches such as DSA <sup>2</sup> still rely on a token-level indexer to identify important positions; as the context grows, the indexer itself becomes a non-negligible source of computation.

QSA further compresses this process: a lightweight indexer first aggregates the sequence into **micro-blocks**, estimates context importance at the block level, and then selects the most relevant regions for Attention. This reduces not only the cost of Attention itself, but also the indexing overhead required to identify important context. Compared with approaches that share indices across layers <sup>3</sup>, QSA performs sequence compression independently within each layer, reducing its dependence on cross-layer Attention similarity and making it particularly well suited to Hybrid architectures where GDN and Attention layers are interleaved.

Put simply: **GDN efficiently “remembers,” while QSA precisely “retrieves.”**

At **1M tokens**, QSA’s Attention Kernel achieves up to **7.6×** and **4.9×** speedups in Prefill and Decode, respectively. In an experimental setup representative of online serving scenarios with high cache reuse (a **90% Prefix Cache hit rate**), Qwen3.8-Flash-Next achieves **8.6×** the Prefill throughput of Qwen3.7-Plus at a **1M-token context length**.

### Gated Residual: More Paths for Information Flow

In a traditional Transformer, all layers continuously read from and write to the same Residual Stream. As the network becomes deeper, early features are repeatedly mixed with later information, making important signals more likely to be gradually diluted.

**Gated Residual (GR)** can be viewed as a combination of two ideas: it follows **Hyper-Connection** <sup>4</sup> in widening the residual stream into multiple branches, while incorporating the element-wise dynamic gating of **GatedNorm** <sup>5</sup> into the residual read. The original single residual stream is expanded into four parallel branches, allowing the model to dynamically determine how much information to read from each branch and how much to write back to each branch based on the current content.

This can be conceptualized as expanding a single information channel into multiple parallel pathways: some branches handle local information flow, while others preserve early information directly deep into the network layers. Empirical analysis also reveals that one of these branches naturally emerges as a long-range pathway connecting the first Attention layer to most of the middle and subsequent layers.

GR also further simplifies Hyper-Connection. Once the read and write operations are expressive enough, additional branch mixing yields no significant benefits and can thus be directly removed, thereby reducing memory access overhead and sources of instability. The Gate also effectively suppresses **activation outliers** and improves training stability. In addition, the Residual State supports **FP8** storage, further reducing memory-access overhead.

### N-gram Embedding: Expanding Model Capacity at Low Cost

Inspired by Per-Layer Embedding in Gemma 3n and works such as DeepSeek Engram <sup>6</sup>, we further introduce **N-gram Embedding** to scale model capacity beyond the parameters of the Transformer backbone.

A standard Embedding performs a lookup based on a single token. N-gram Embedding instead performs lookups using the local context formed by the current token and several preceding tokens, providing additional representations for common phrases and local patterns.

Its key advantage is that it **can add a large number of parameters with almost no additional computation per token**.

Qwen3.8-Flash-Next introduces an additional **51B N-gram Embedding parameters**. Because lookup locations can be determined in advance, these parameters can be stored in Host Memory and asynchronously prefetched in parallel with model computation, without permanently occupying GPU memory.

The final model uses only a single N-gram Embedding layer near the beginning of the network, effectively adding a large-scale **“local-pattern memory”** at relatively low additional cost.

### Optimization: Co-designing Architecture and Optimization

Qwen3.8-Flash-Next is trained with the **Muon Optimizer** <sup>7</sup>, with further improvements around three key aspects of applying Muon to large-scale model training: **orthogonalization accuracy, parameter assignment between Muon and AdamW, and splitting fused parameter matrices**.

For parameters that genuinely act as two-dimensional linear maps, such as the main weights in Attention, GDN, and MoE Experts, we use Muon. Embeddings, the MoE Router, and the low-rank parameters in GR continue to use AdamW. For QKV, SwiGLU, and GDN projections that are fused in the implementation, we first split them according to the independent linear transformations they represent, and then perform orthogonalization separately.

For the new architecture and Optimizer, we refit the Scaling Law. The results show that the model can stably use **larger Learning Rates and Batch Sizes**, further improving convergence efficiency and large-scale parallel training throughput.

We also find that **Batch Size Warmup**, a common practice in large-scale model training, is no longer necessary: gradually increasing from a small Batch to the target Batch does not improve the final result, but instead requires **18.8% more optimizer steps**. In the final training Recipe, we therefore start directly with the target Batch Size.

### Other Architecture Optimizations

The remaining components follow the design established in Qwen3-Next and refined through the Qwen3.5–Qwen3.8 series.

- 
**Ultra-sparse MoE** : With global load balancing<sup>8</sup> , increasing total expert parameters while keeping the number of activated experts fixed steadily reduces training loss. Qwen3.8-Flash-Next therefore uses a large expert pool with a small number of routed experts per token, together with one shared expert.
- 
**Multi-Token Prediction** : The MTP module is trained with multiple steps, maintaining consistency between training and inference and thereby improving the acceptance rate of speculative decoding in real scenarios, while also enhancing the performance of the backbone. Its full-attention layers are replaced with QSA as well.
- 
**Training stability** : Zero-centered RMSNorm with weight decay applied to norm weights, the attention output gating mechanism<sup>9</sup> , and normalized MoE router initialization are retained. These designs make small-scale ablations more reliable and help large-scale training run smoothly.

## Base Model Performance

We compare Qwen3.8-Flash-Next-Base with the base models of Qwen3.8-27B and Qwen3.7-Plus.

With 6B activated parameters, Qwen3.8-Flash-Next-Base achieves the best result on 8 of the 14 benchmarks, including MMLU-Pro, SuperGPQA, BBH, GSM8K, EvalPlus, SWEBench-Pretrain, MGSM and MMMLU, and remains close to Qwen3.7-Plus-Base on MMLU, MMLU-Redux, GPQA, MATH and MultiPL-E. The 51B N-gram embedding parameters are deterministically addressed and do not enter the per-token matrix-multiplication budget.

## Develop with Qwen3.8-Flash-Next

Qwen3.8-Flash-Next is available as an open-weight model on HuggingFace and ModelScope, with official managed APIs on QwenCloud. Designed to balance capability, latency, and cost, it is well suited for high-volume applications, tool-driven workflows, and coding & coworking assistants. In the following, you can explore how to call the QwenCloud API and integrate Qwen3.8-Flash-Next into agentic systems and coding assistants.

### API Usage

Qwen3.8-Flash-Next is available via API:

#### QwenCloud

On QwenCloud, the model is served under the name qwen3.8-flash. QwenCloud supports industry-standard protocols, including OpenAI-compatible Chat Completions and Responses APIs, alongside an Anthropic-compatible interface.

```
"""
Environment variables:
  DASHSCOPE_API_KEY: Your API Key from https://home.qwencloud.com/
  DASHSCOPE_BASE_URL: (optional) Base URL for compatible-mode API.
    - Beijing: https://dashscope.aliyuncs.com/compatible-mode/v1
    - Singapore: https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    - US (Virginia): https://dashscope-us.aliyuncs.com/compatible-mode/v1
"""
from openai import OpenAI
import os
api_key = os.environ.get("DASHSCOPE_API_KEY")
if not api_key:
    raise ValueError(
        "DASHSCOPE_API_KEY is required. "
        "Set it via: export DASHSCOPE_API_KEY='your-api-key'"
    )
client = OpenAI(
    api_key=api_key,
    base_url=os.environ.get(
        "DASHSCOPE_BASE_URL",
        "https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
    ),
)
messages = [{"role": "user", "content": "Write a Python function to merge two sorted linked lists."}]
completion = client.chat.completions.create(
    model="qwen3.8-flash",
    messages=messages,
    extra_body={
        "enable_thinking": True,
        # "preserve_thinking": True,
    },
    reasoning_effort="xhigh",  # supported levels are xhigh, medium, and low
    stream=True,
)
reasoning_content = ""
answer_content = ""
is_answering = False
print("\n" + "=" * 20 + "Reasoning" + "=" * 20 + "\n")
for chunk in completion:
    if not chunk.choices:
        print("\nUsage:")
        print(chunk.usage)
        continue
    delta = chunk.choices[0].delta
    if hasattr(delta, "reasoning_content") and delta.reasoning_content is not None:
        if not is_answering:
            print(delta.reasoning_content, end="", flush=True)
        reasoning_content += delta.reasoning_content
    if hasattr(delta, "content") and delta.content:
        if not is_answering:
            print("\n" + "=" * 20 + "Answer" + "=" * 20 + "\n")
            is_answering = True
        print(delta.content, end="", flush=True)
        answer_content += delta.content
```
### Agent Frameworks & Coding Assistants

Qwen3.8-Flash-Next integrates seamlessly with popular agent frameworks and coding assistants:

#### QwenWork (Coming Soon)

QwenWork is Alibaba’s flagship AI productivity platform, designed to help individuals and enterprises automate daily tasks and accelerate operational efficiency.

We are excited to share that QwenWork has integrated **Qwen3.8-Flash-Next** to power its newly launched “Standard” mode, leveraging the model’s cutting-edge capabilities to deliver a seamless, cost-effective experience that sets a new standard for AI agents in the workplace.

Learn more in the official documentation!

#### Claude Code

Qwen APIs support the Anthropic API protocol, enabling direct use with **Claude Code**:

```
npm install -g @anthropic-ai/claude-code
export ANTHROPIC_MODEL="qwen3.8-flash"
export ANTHROPIC_SMALL_FAST_MODEL="qwen3.8-flash"
export ANTHROPIC_BASE_URL=https://dashscope-intl.aliyuncs.com/apps/anthropic
export ANTHROPIC_AUTH_TOKEN=<your_api_key>
claude
```
#### Codex

Qwen APIs support the OpenAI Responses protocol, enabling use with **Codex**:

In `~/.codex/model-catalog.local.json`

```
{
  "models": [
    {
      "slug": "qwen3.8-flash",
      "display_name": "qwen3.8-flash",
      "description": "QwenCloud: Qwen3.8-Flash",
      "default_reasoning_level": "xhigh",
      "supported_reasoning_levels": [
        {
          "effort": "low",
          "description": "Fast responses with lighter reasoning"
        },
        {
          "effort": "medium",
          "description": "Greater reasoning depth for complex problems"
        },
        {
          "effort": "xhigh",
          "description": "Extra high reasoning depth for complex problems"
        }
      ],
      "context_window": 1000000,
      "effective_context_window_percent": 95,
      "supports_parallel_tool_calls": true,
      "supports_image_detail_original": true,
      "input_modalities": ["text", "image"],
      "shell_type": "default",
      "visibility": "list",
      "supported_in_api": true,
      "priority": 1,
      "base_instructions": "",
      "support_verbosity": false,
      "supports_reasoning_summaries": false,
      "experimental_supported_tools": [],
      "truncation_policy": {
        "mode": "bytes",
        "limit": 10000
      }
    }
  ]
}
```
In `~/.codex/config.toml`

```
model_catalog_json = "~/.codex/model-catalog.local.json"
model_provider = "QwenCloud"
model = "qwen3.8-flash"
[model_providers.QwenCloud]
name = "QwenCloud"
base_url = "https://dashscope-intl.aliyuncs.com/compatible-mode/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```
```
npm install -g @openai/codex
export OPENAI_API_KEY=<your_api_key>
codex
```
#### Qoder CLI

Qoder co-evolves with Qwen for agentic coding:

```
curl -fsSL https://qoder.com/install | bash
qoder
```
#### Qwen Code

Qwen Code is deeply optimized for the Qwen series:

```
npm install -g @qwen-code/qwen-code@latest
qwen
```
#### OpenClaw

Connect to OpenClaw via QwenCloud:

```
curl -fsSL https://openclaw.ai/install.sh | bash
export DASHSCOPE_API_KEY=<your_api_key>
openclaw dashboard
```
Configure `~/.openclaw/openclaw.json`:

```
{
  "models": {
    "mode": "merge",
    "providers": {
      "qwencloud": {
        "baseUrl": "https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
        "apiKey": "DASHSCOPE_API_KEY",
        "api": "openai-completions",
        "models": [
          {
            "id": "qwen3.8-flash",
            "name": "qwen3.8-flash",
            "reasoning": true,
            "input": ["text", "image"],
            "contextWindow": 1000000,
            "maxTokens": 65536
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "qwencloud/qwen3.8-flash"
      }
    }
  }
}
```
## Summary

Qwen3.8-Flash-Next extends the hybrid architecture introduced in Qwen3-Next along four directions: attention, residual, embedding and optimization. QSA compresses the sequence into micro-blocks within each layer, reducing both the attention cost and the indexing cost at long context while keeping precise retrieval. Gated Residual widens the residual stream into several parallel branches and controls reads and writes with an elementwise, data-dependent gate, improving cross-layer information flow and training stability at negligible arithmetic cost; the residual state can additionally be kept in FP8, which further reduces memory traffic. N-gram embedding scales capacity through deterministically addressed lookup memory, which can be scaled with negligible per-token computation and offloaded to host memory. On the optimization side, Muon is used as the main optimizer, with orthogonalization accuracy, parameter assignment and fused-matrix splitting as the decisive implementation choices, and the scaling law refitted for the new architecture.

We release these weights early so that the architecture can be evaluated independently by the community, as we did with Qwen3-Next, and we will continue to refine it towards Qwen4.

## Citation

```
@techreport{qwen2026design,
    title       = {On the Design of {Qwen3.8-Next} Architecture: Evaluation, Efficiency, and Training Stability},
    author      = {{Qwen Team}},
    institution = {Alibaba Group},
    month       = {August},
    year        = {2026}
}
@misc{qwen3.8flashnext,
    title  = {{Qwen3.8-Flash-Next}: A New Architecture, Towards Ultimate Cost-Efficiency},
    author = {{Qwen Team}},
    month  = {August},
    year   = {2026},
    url    = {https://qwen.ai/blog?id=qwen3.8-flash-next}
}
```
# References

[1] Gated Delta Networks: Improving Mamba2 with Delta Rule

[2] DeepSeek-V3.2: Pushing the Frontier of Open Large Language Models

[3] IndexCache: Accelerating Sparse Attention via Cross-Layer Index Reuse

[4] Hyper-Connections

[5] A Unified View of Attention and Residual Sinks: Outlier-Driven Rescaling is Essential for Transformer Training

[6] Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models

[7] Muon: An Optimizer for Hidden Layers in Neural Networks

[8] Demons in the Detail: On Implementing Load Balancing Loss for Training Specialized Mixture-of-Expert Models

[9] Gated Attention for Large Language Models: Non-linearity, Sparsity, and Attention-Sink-Free

## Read previous post:

## Read next post:

### Comments

# Related Products

- 
                            
                            Build more, spend less. One plan, every modality. Learn More
- 
                            
                            QwenWork is dedicated to helping employees strengthen their professional competitiveness in the AI era and to enabling enterprises to improve organizational effectiveness. Learn More
- 
                            
                            A one-stop generative AI platform to build intelligent applications that understand your business, based on Qwen model series such as Qwen-Max and other popular models Learn More
- 
                            
                            Full-range, open-source, multimodal, and multi-functional Learn More
