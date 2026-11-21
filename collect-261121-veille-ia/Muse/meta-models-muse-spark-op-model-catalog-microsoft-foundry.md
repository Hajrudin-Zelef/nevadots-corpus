---
id: collect-261121-veille-ia/muse/meta-models-muse-spark-op-model-catalog-microsoft-foundry
title: "meta-models--muse-spark-op... | Model Catalog | Microsoft Foundry"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["model", "open", "muse", "spark", "output", "attention", "inference", "license"]
source: https://ai.azure.com/catalog/models/meta-models--muse-spark-open-816b-a42b-nvfp4?search=muse
source_anchor: ""
source_lines: [1, 98]
section: "Muse"
---
# meta-models--muse-spark-open-816b-a42b-nvfp4

### Muse Spark Open Model Card

**Authors**: Meta Superintelligence Labs

**Model Release Date:** October 2026

**License**: Muse Model License 

Muse Spark Open is an 816B total/42B active parameter open weights model with a context window of 1M tokens. Built for coding and agentic work, Muse Spark Open is natively multimodal. This open weights version has additional safety post-training to make it more robust against misuse as a standalone model without system guardrails. We recommend deploying Muse Spark Open as part of a broader AI system, with additional guardrails suited to the specific use case.

The model can perform well across a wide variety of use cases including:

1. **Multi-Step Reasoning** - chains reasoning over long horizons, sustaining coherent plans across complex, extended workflows.
2. **End to End Agentic Task Completion** - multi-step planning, sequential tool invocation, failure recovery, and long-horizon task execution, working within scaffolds, write and debug code, and resolve multi-turn requests from start to finish.
3. **Coding Agents** - writing, debugging, and resolving real-world software engineering tasks.
4. **Tool Use** - tool invocation across extended multi-turn workflows, diagnosing error and retrying with tool call failures
5. **Multimodal Reasoning** - interpreting screenshots, charts, documents, images, audio and video recordings, alongside conversation for agentic and information-rich environments.
6. **Synthetic Data Generation** - generating high-quality training data for downstream model development.
7. **LLM-as-a-judge Evaluation** - serving as an evaluator for other models' outputs.

#### Muse Spark Open Model Overview

| Name | Muse Spark Open | 
|---|---|
| Model Provider | Meta Platforms, Inc. or Meta Platforms Ireland Limited | 
| Date of Release | October 15th, 2026 | 
| License | Muse Model License | 
| Parameters | 816B total / 42B active | 
| Type | Mixture-of-Experts Transformer, with Perception Encoder | 
| Context Length | 1M at inference | 
| Vocabulary Size | 202,048 | 
| Modalities | Input: Text + Image + Video + Audio / Output: Text | 
| Languages | Multilingual, trained on data from more than 100 languages | 
| Recommended inference parameters | Temperature = 1.0 Top_p = 1.0 Top_k = disabled It is recommended to use Reasoning Effort of high or xhigh for complex problem solving, coding, and agentic tasks. | 
| Language Model | **Hidden Dimension:** 8192**Token Embedding:** untied input/output embeddings, vocab × dim = 202,048 × 8192 ≈ ~1.66B**Output layer (unembedding layer):** vocab × dim = 202,048 × 8192 ≈ ~1.66B**Number of Layers:** 62**Gated Attention:** Yes (output gate and output norm on the attention block; QK-norm enabled) Number of Attention Heads: 128 for Q and 16 for KV (grouped-query attention) Head Dimension: 64**Mixture of Experts:** Yes Number of Experts: 256 Experts Activated per Token: 8 (top-k routing) | 
| Perception Encoder | **Architecture:** 1.8B param, 50 layers, 1536 hidden dim, 16 heads, 14x14 patch size, 4096 output dim Learned Absolute + 2D RoPE for positional embedding, LayerNorm + GELU for FFN Sparse attention with factor 4 (every 4th + last layer are full attention) | 

#### Model Access and Best Practices

**Access via API**

The model is available via API through third party inference providers.

**Open Weights on your own system**

The weights are available for download through Hugging Face. The model is available in BF16 and NVFP4  versions. To run on your own system, the model requires a GPU with at least 700GB of VRAM for NVFP4 and 2TB for BF16. It also requires an inference deployment framework - supported frameworks are SGLang, vLLM, Hugging Face, TensorRT-LLM, Unsloth and Llama.cpp. It is also optimized to run on two Mac Studios with M5 Ultra using MLX.

Please see the developer documentation for more information on how to deploy and use the model.

**Inference Parameters**

You can customize inference parameters to achieve the best performance for your use case.

- **Reasoning Effort:** Reasoning effort controls how much the model thinks before it responds. Set it with the reasoning_effort request parameter, or with chat_template_kwargs={"reasoning_effort": ...}, to low, medium, high, or xhigh. The default is xhigh. Use high or xhigh for complex problem solving, coding, and agentic tasks, and low or medium for faster, shorter responses. Any other value returns an error.
- **Sampling Parameters:** The following settings are recommended for most deployments: temperature = 1.0, top_p = 1.0, top_k = disabled

#### Training data

Training data includes text, images, audio, and video. It includes data drawn from public sources, crawled from the Internet, acquired from third parties, derived from Meta’s products and services, and synthetically generated, curated and enriched by external vendor networks and Meta personnel. Data curation methodologies include cleaning, processing, modifying, deduplication, and filtering, among others.

#### Safety and Preparedness

We strongly recommend deploying Muse Spark Open as part of an AI system with guardrails tailored to your intended use cases. These system-level protections should balance helpfulness and safety and address safety and security risks, including those arising from integration with external tools.

Muse Spark Open has been evaluated across four complementary safety areas, and met our safety criteria for release based on model evaluations and an assessment of residual risks:

1. Content safety: Harmful-content generation and refusal behavior, alongside response quality and ideological balance.
2. Agentic risk: Harmful or unauthorized actions when using tools or acting on users' behalf, including unsafe code execution and destructive changes in coding-agent workflows.
3. Privacy: Evaluation of how the model handles personal and sensitive information, including inappropriate disclosure and information sharing when acting on users' behalf.
4. Preparedness: Evaluation of chemical and biological, cyber, and loss-of-control risks in line with  the Meta Superintelligence Scaling Framework.
  1. Cybersecurity: We tested it against internal and public cyber misuse benchmarks using adaptive attackers querying the model the way a developer would query an API model, with no system-level safeguards in place. The model refused 99.2% of jailbreaks on our internal benchmark and 94.8% when aggregating across PolyGuard (cyber subset), RedCode, and CyberBench 2024. In the same setup, Kimi K3 refused only 75.5% and 70.2% respectively.
  2. Chemical and biological: It showed strong refusal behavior on both single-turn and multi-turn benchmarks, refusing over 98% of harmful requests on SecureBio's BioTIER refusal benchmark and over 99% on our internal single-turn chemistry refusal benchmark. On newly developed internal multi-turn agentic benchmarks, it refused over 97% of harmful biology requests and 98% of harmful chemistry requests.
  3. Loss of control: On our misalignment evaluations, the model showed low propensity for deceptive or unilateral harmful behavior, with a 98.1% honesty rate on the MASK benchmark and a 0% misbehavior rate on the Agentic Misalignment benchmark.

These evaluations characterized the released checkpoint under the tested conditions and do not cover every downstream configuration or use case. Downstream modifications could degrade safety, and results for the released checkpoint should not be assumed to apply to modified models.

#### Considerations and Limitations

Muse Spark Open is a technology that carries known and unknown risks. Testing conducted to date has not, and could not, cover all scenarios. Muse Spark Open must not be used in any manner that violates applicable laws or regulations (including trade compliance laws) or in any other way that is prohibited by the Muse Model License , including the Usage Policy.

**Limitations**

1. The model may produce inaccurate, biased, or objectionable responses to user prompts.
2. While optimized for agentic tasks, the model may still make errors in multi-step reasoning, particularly in novel scenarios not well represented in training data.
3. The model has not been evaluated on all languages contained in the pre-training data. Performance may degrade on languages outside the strongly supported set.
4. Quantized inference may show minor quality differences in edge cases compared to full-precision.
5. The model is not intended to be downloaded by or used by individuals under the age of 18. Where deployed within systems that may be used by individuals under the age of 18, deployers are responsible for ensuring that any risks associated with such use by individuals under the age of 18 has been fully assessed and appropriately mitigated, and complies with all applicable laws.
6. When using very short or low-resolution videos (a few seconds at around 100 px), the model may sometimes reply that it can't view the video. Use longer clips or higher resolution (448 px or more on the short side) for reliable results.

**Responsible Use**

Developers should perform their own safety testing and tuning tailored to their specific applications and proposed languages. Our Usage Policy can be found in the Muse Model License here  and provides further information on intended and unintended uses. We recommend implementing additional guardrails (such as human-in-the-loop confirmation for irreversible actions) when deploying the model in agentic contexts where it can take real-world actions.

For user-facing applications, developers should avoid exposing raw chain-of-thought and apply appropriate safety checks to any reasoning summaries. Developers should also protect internal assistant messages from user manipulation and use layered safeguards against jailbreaks and other adversarial inputs.

**Where to send questions or comments about the model:** Please provide any feedback, comments or bug reports on the model through the Hugging Face page at https://huggingface.co/meta-models/  or through email at museoss@meta.com . For more technical information about generation parameters and recipes for how to use Muse Spark Open in applications, please see the developer documentation.
