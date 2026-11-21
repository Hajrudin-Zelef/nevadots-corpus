---
id: collect-261121-veille-ia/modeles-open/reflection-beam-1-0-501b-a23b-nvfp4-hugging-face
title: "reflection/Beam-1.0-501B-A23B-NVFP4 · Hugging Face"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-06-30"]
keywords: ["beam", "reasoning", "reflection", "agentic", "hugging", "face", "text", "parameters"]
source: https://huggingface.co/reflection/Beam-1.0-501B-A23B-NVFP4
source_anchor: ""
source_lines: [1, 98]
section: "Modeles_Open"
---
Hugging Face |
  GitHub |
  Launch Blog |
  Documentation |
  Technical Report (coming)
  

  **License**: Apache 2.0 |
  **Authors**: Reflection

Beam is an open model built by Reflection. Beam takes text as input and generates text as output. This checkpoint contains the NVFP4-quantized instruction-tuned model. The Beam release also includes BF16 and FP8 checkpoints. Beam uses a sparse mixture-of-experts (MoE) architecture with 501B total parameters and 23B active parameters. It supports a context window of 1M tokens and was developed predominantly for English.

- **Token-efficient reasoning:** Reaches comparable solution quality to leading open models using approximately 3x fewer inference-time reasoning tokens.
- **Software engineering:** Resolves real-world GitHub issues, operates in CLI and DevOps environments, and integrates directly with agentic coding harnesses. Code generation supported across languages.
- **Agentic tool use:** Long-horizon task completion, MCP-based tool integration, and browsing. Executes multi-step workflows from natural-language instructions.
- **Reasoning (math and science):** Graduate-level science problem solving, theorem proving, and applied mathematics. Reasoning quality transfers to debugging, planning, and self-correction during agentic execution.

Beam models are designed to deliver strong performance for areas such as agentic tool use, coding and general reasoning with an emphasis on token efficient usage.

| Property | Beam 501B A23B MoE (NVFP4) | 
|---|---|
| Total Parameters | 501.3B | 
| Active Parameters | 23.4B | 
| Layers | 52: 3 dense + 49 MoE | 
| Attention | Gated GQA + Sliding Window Attention | 
| Context Length | 1M | 
| Vocabulary Size | 152k tokens | 
| Expert Count | 256 | 
| Supported Modalities | Text | 
| Chat Template | Modified Harmony format | 
| Numerics | NVFP4 | 
| Knowledge cutoff | June 30, 2026 | 

These models were evaluated against a large collection of different datasets and metrics to cover different aspects of text generation. Evaluation results marked in the table are for instruction-tuned models spanning all released numerics. The green-highlighted column identifies this NVFP4 checkpoint. Note: All evaluations were done using temp=0.7, top_p=0.99.

| Benchmark | Beam (BF16) | Beam (FP8) | Beam (NVFP4) | 
|---|---|---|---|
| Agentic Coding / Terminal |  |  |  | 
| DeepSWE v1.1 | 43.3% | 43.0% | 42.7% | 
| SWE Bench Pro v2-Hard | 76.5% | 78.4% | 78.6% | 
| SWE Bench Pro v1 | 66.1% | 65.9% | 65.6% | 
| Terminal Bench v2.1 | 80.1% | 78.9% | 78.4% | 
| SWE Atlas Codebase QnA | 31.8% | 32.2% | 31.4% | 
| SWEBench Multilingual | 78.3% | 78.1% | 77.5% | 
| SWEBench Verified | 81.6% | 80.2% | 80.7% | 
| Reasoning |  |  |  | 
| AIME 2026 | 97.7% | 98.0% | 97.5% | 
| HLE (no tools) | 36.0% | 36.0% | 34.8% | 
| HLE (with tools) | 51.1% | 51.1% | 51.1% | 
| SciCode | 49.2% | 48.3% | 46.9% | 
| CriPT AA | 16.3% | 17.4% | 15.4% | 
| GPQA Diamond | 90.6% | 90.3% | 89.3% | 
| LiveCodeBench | 88.3% | 88.1% | 88.3% | 
| Tool Calling / Search |  |  |  | 
| AutomationBench - public split only | 39.3% | 38.2% | 39.8% | 
| MCP Atlas | 80.8% | 81.5% | 80.5% | 
| Tau3 Banking | 36.1% | 36.8% | 35.4% | 
| BrowseComp w/ context management | 76.8% | 76.7% | 75.6% | 
| DeepSearchQA w/ context management | 80.5% | 79.6% | 81.1% | 
| General Capabilities |  |  |  | 
| AA-LCR | 78.2% | 79.4% | 78.7% | 
| LongBench v2 | 67.5% | 67.6% | 67.2% | 
| IFBench | 79.6% | 80.7% | 79.2% | 
| AA Omniscience (index) - public split only | 10.8 | 8.8 | 9.8 | 

See the evaluation guide for recommended settings, Beam Verifier and a SWE-bench Verified example.

Beam is pretrained on a diverse variety of textual data from publicly available sources and data obtained through commercial partner agreements. The corpus includes general web content, source code, technical documentation, reference materials, and educational and scientific documents. Mid-training uses data derived from curated real-world examples to develop reasoning, tool-use, and long-context capabilities, including structured code repositories, long-horizon tasks, and long-form documents. Reinforcement learning uses environments spanning coding, agentic tasks, and STEM, built primarily through synthetic pipelines and supplemented by proprietary vendor data and open-source sources.

Data processing includes quality filtering, deduplication, and evaluation decontamination to reduce benchmark contamination. For all types of pretraining data, we measure document quality with in-house trained classifiers, divide data into fine-grained quality tiers, and weigh training toward stronger material. We perform multiple forms of deduplication, exact and approximate. We curate code data via language-specific filtration and removal of low-quality and autogenerated content. We also transform educational material from PDF form into textual training data using commercially available vision-language OCR models and a curation pipeline to filter low-quality artifacts. RL tasks are iteratively filtered for quality and difficulty. We also curate datasets to reduce personally identifiable information and harmful or sensitive content.

Before releasing Beam, we ran a comprehensive set of safety evaluations to measure its performance and identify potential risks requiring mitigation. We run both Open Safety Framework evaluations, external third party assurance evaluations, and The Iris Safety Evaluations (ISE), which is Reflection’s in-house soon-to-be open-sourced safety evaluation suite, combining single-turn, multi-turn, jailbreak, and agentic evaluations to measure adherence to our safety policies. Learn more about our Safety & Alignment methodology and the detailed results of how we’ve evaluated Beam at reflection.ai/beam-safety-report.

| Benchmark | Beam (BF16) | Beam (FP8) | Beam (NVFP4) | 
|---|---|---|---|
| Iris Safety Evaluations (Combined All Harm Areas) |  |  |  | 
| Harmful Requests: Refusal/Pivot Rate | 88.9% | 89.7% | 88.5% | 
| Harmless Requests: Compliance Rate | 81.5% | 81.6% | 82.1% | 

Beam is a text model intended for commercial and research use, including applications such as assistant-style generation, software engineering and agentic coding tasks, long-horizon agentic tasks and summarization, and as a base model for fine-tuning. Reflection does not prohibit distillation, use of outputs to train other models, fine-tuning, parameter-efficient adaptation, quantization, merging, defensive security research on systems you are authorized to test, and good-faith adversarial evaluation of the model.

When building with this model, apply responsible AI practices and verify your use case meets relevant legal requirements (privacy, export compliance, and others). Our Usage Policy lists the areas we recommend reviewing. Note: this Model Card is informational only and does not modify or restrict the model's license.

Report suspected misuse, safety concerns or security vulnerabilities to reports@reflection.ai.

If you find our work helpful, please consider citing it:

```
@misc{reflection2026introducingbeam,
  author = {{Reflection}},
  title  = {Introducing {Beam}: {Reflection}'s 501B open-weight model},
  year   = {2026},
  month  = oct,
  url    = {https://reflection.ai/blog/introducing-beam}
}
```
- Downloads last month
- 5,360
