---
id: collect-261121-veille-ia/mistral/mistral-ships-1t-param-large-4-le-chonk-open-weight
title: "Mistral Ships 1T-Param Large 4: Le Chonk Open-Weight"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "model", "open-weight", "large", "chonk", "parameters", "hugging", "face"]
source: https://sutopo.com/mistral-ships-1t-param-large-4/
source_anchor: ""
source_lines: [1, 93]
section: "Mistral"
---
- Mistral Large 4 (Le Chonk) gives practitioners a 1-trillion-parameter open-weight MoE model they can self-host, with roughly 49 to 52 billion active parameters per token per Mistral AI and Hugging Face (2026)
- Native multimodal support and a 1-million-token context window are built in, covering text, images, and long-document workloads per Hugging Face (2026)
- Two quantized checkpoints ship with the model: FP8 for precision-critical hardware and NVFP4 for memory-constrained setups
- API pricing via la Plateforme is $0.70 per million input tokens and $2.10 per million output tokens per Mistral AI (2026)
- The open-weight license permits commercial use, but it is not full open source; reading the license before production deployment is mandatory

Mistral ships 1T-param weights as a freely downloadable open-weight release, and the AI community has already given the latest one a fitting nickname: Le Chonk. Mistral Large 4 is a natively multimodal model with 1 trillion total parameters and approximately 49 to 52 billion active parameters per forward pass, according to Mistral AI and Hugging Face (2026). It is built on a Mixture-of-Experts (MoE) architecture that activates only a small fraction of the total network per token, which keeps inference costs considerably lower than a comparably sized dense model. The 1-million-token context window and built-in image understanding make it practical for code analysis, long-document pipelines, and security tooling rather than just chat benchmarks.

The model lands in two forms: open weights on Hugging Face in FP8 and NVFP4 checkpoints, and managed API access through Mistral’s la Plateforme. This article covers what the model is, how the MoE architecture distributes compute, what the license actually permits, and how to get a working deployment from beginner-level setup through production-grade serving.

- Download weights from the official Hugging Face repository; FP8 and NVFP4 checkpoints target different hardware memory budgets
- Supported inference frameworks include Transformers v5.22.0+, vLLM, SGLang, and llama.cpp
- Cached input tokens on la Plateforme cost $0.07 per million, making repeated-context workloads significantly cheaper per Mistral AI (2026)
- Read the open-weight license before redistributing weights or deploying in regulated domains

Mistral Large 4 is a natively multimodal open-weight language model with 1 trillion total parameters, designed for instruction following, long-context reasoning, code generation, and vision tasks. That definition needs context: “open weight” means the trained model weights are freely downloadable and commercially usable, but Mistral has not released training data or internal tooling. The Le Chonk nickname surfaced in developer communities immediately after the October 2026 release, referencing the headline parameter count in the most direct way possible.

Mistral AI reports 52 billion active parameters per token, while the Hugging Face model card for the same release puts that figure at approximately 49 billion active parameters. Both sources confirm roughly 5 percent of the total network activates per inference step. The small discrepancy reflects different rounding conventions across published figures rather than any technical conflict.

The model explicitly targets a broad workload range: multilingual text, structured data extraction, code writing and review, image understanding, and reasoning-heavy tasks. TechCrunch reported that Mistral specifically names cybersecurity as a target domain, pointing to log analysis and threat investigation as strong fits for the deep context window.

API access through la Plateforme costs $0.70 per million input tokens, $0.07 per million cached input tokens, and $2.10 per million output tokens per Mistral AI (2026). For teams whose queries reuse long system prompts, the cached input pricing is the number to watch.

When Mistral ships 1T-param models with an open-weight license, memory and compute requirements become the central question. The total parameter count is 1 trillion per Mistral AI (2026), but active parameters per token sit at roughly 49 to 52 billion. For infrastructure planning, active parameters drive compute cost; total parameters drive memory footprint.

Two checkpoint formats are available. FP8 stores each weight in 8-bit floating point, keeping approximately half the memory footprint of a full FP16 model (8 bits divided by 16 bits FP16 baseline). NVFP4 uses 4-bit weights optimized for NVIDIA hardware, reducing footprint to roughly one quarter of FP16 cost (4 divided by 16). The tradeoff is numerical precision: FP8 preserves more of the full-precision characteristics; NVFP4 trades some precision for a larger memory margin.

| Checkpoint | Bits per weight | Memory vs FP16 baseline | Best suited for | 
|---|---|---|---|
| FP8 | 8 | ~50% of FP16 | Data-center GPUs (H100, A100); precision-sensitive workloads | 
| NVFP4 | 4 | ~25% of FP16 | Memory-constrained setups; multi-GPU consumer hardware | 

Memory ratios derived from bits-per-weight divided by 16-bit FP16 baseline. Consult the Hugging Face model card for exact GPU configuration requirements. KV-cache memory adds to weight memory at long context lengths.

Supported inference stacks include Transformers (from v5.22.0 per the official release notes), vLLM, SGLang, and llama.cpp. Each framework handles batching and tensor parallelism differently, so the right choice depends on expected query volume and hardware topology.

A Mixture-of-Experts architecture splits the network into many specialist sub-networks and routes each token through only a small subset of them. For Mistral Large 4, the router activates roughly 49 to 52 billion parameters per token even though the full model contains 1 trillion. The model stores a large volume of specialized knowledge without paying the full compute cost of a dense 1T network on every forward pass.

For practitioners, the practical implication is that throughput per token resembles that of a roughly 50B dense model once the full weights are resident in GPU memory. The hardware bottleneck shifts from floating-point compute to memory bandwidth, since all 1 trillion parameters still need to be addressable even if only a fraction fires per token.

This architecture directly shapes deployment strategy. Single-machine setups benefit most from the NVFP4 checkpoint, which brings the per-GPU memory requirement low enough that a multi-GPU consumer node becomes viable. Multi-node serving with NVLink-connected data-center GPUs can run the FP8 checkpoint with better throughput at scale using tensor parallelism across devices. Disaggregated prefill-decode configurations in SGLang are worth evaluating for latency-sensitive APIs where prefill and decode phases have different optimal hardware profiles.

The routing mechanism is learned during training rather than manually assigned. Code generation, multilingual translation, and mathematical reasoning tend to activate different expert subsets consistently (as is typical in MoE architectures), giving the model implicit domain specialization without requiring separate fine-tuned variants per task.

Mistral Large 4 accepts text and images as native inputs without a separate vision adapter or preprocessing pipeline. Image tokens are processed inline with text tokens, which allows natural prompting patterns such as passing a screenshot alongside a debugging question. The multimodal capability covers tasks like diagram interpretation, chart reading, and visual code review in a single model call.

The context window reaches 1 million tokens per Hugging Face (2026). That accommodates entire codebases, long regulatory documents, or multi-session conversation histories without truncation. Many production models cap well below the million-token range, so the gap is practically significant for teams building retrieval-free long-document workflows.

Long-context inference carries real memory costs. KV-cache memory scales linearly with sequence length, adding to the weight memory from the checkpoint format. Teams planning to push close to the 1M-token limit should profile actual memory consumption on representative inputs before locking in a hardware configuration; the difference between a 100K-token and a 1M-token workload is substantial in peak VRAM terms.

**💡 Pro Tip:**When testing at long context lengths, start at 10 to 20 percent of your expected maximum and measure KV-cache memory growth per additional 100K tokens. That growth rate lets you project peak memory and avoid OOM failures before they hit in production.

A reasoning mode is available as a per-request setting, triggering step-by-step internal traces before the final answer. Useful for math, code debugging, and logical inference, this mode adds latency overhead on simpler tasks, so activating it selectively rather than globally keeps throughput reasonable for mixed workloads.

The open-weight license that accompanies the Mistral ships 1T-param release puts this model in a different category from closed API-only systems. Weights are freely downloadable and usable for building commercial products, which is a meaningful practical distinction for teams that need on-premises deployment or data-residency guarantees.

“Open weight” and “open source” are not synonyms here. The model weights are accessible; the training data, architecture design documentation, and internal tooling are not necessarily disclosed. Practitioners should not assume full transparency about training procedures when making claims about the model’s behavior in regulated or high-stakes settings.

Before any commercial deployment, read the actual license text posted alongside the model card. The areas that most often require legal review are: redistribution rights (can you host a derivative for third parties?), attribution requirements, and any use-case restrictions around high-risk domains like healthcare diagnostics or autonomous security systems.

API access via la Plateforme operates under Mistral’s service terms rather than the open-weight license, which changes the legal relationship and removes direct control over weights. For privacy-sensitive workloads or regulated industries, the self-hosted path under the open-weight license typically offers more operational control, with the tradeoff of managing infrastructure.

**💡 Pro Tip:**Run the license text through your team’s standard open-weight checklist before integrating into a customer-facing product. Redistribution and high-risk domain clauses are where permissive-sounding licenses often include restrictions that are easy to overlook.

**Beginner:** Read the official model card and the license file before downloading anything. Select the NVFP4 checkpoint if working on a multi-GPU consumer setup; it requires roughly 25 percent of the FP16 memory footprint. Install Transformers v5.22.0 or later and use `from_pretrained` with the official Hugging Face repository to pull weights. Verify the installation with a short text prompt before attempting multimodal or long-context inputs.

**Intermediate:** Move from Transformers to vLLM or SGLang for production serving. Set `--tensor-parallel-size` to match your GPU count and enable the vision pipeline flag only if your workload requires image input, as it adds memory overhead for text-only use cases. Set `max_model_len` explicitly to your realistic maximum rather than defaulting to 1M tokens; KV-cache allocation is determined at startup and wastes memory if set far above actual usage.

**Advanced:** Benchmark FP8 against NVFP4 on your actual task distribution: measure tokens per second, peak VRAM, and accuracy on a held-out eval set that reflects production traffic. Evaluate disaggregated prefill-decode serving in SGLang for latency-sensitive APIs. Activate reasoning mode selectively per request type rather than globally, and calculate cost-per-output-token against the la Plateforme rate of $2.10 per million output tokens per Mistral AI (2026) to identify the crossover point where self-hosted hardware pays off over API billing.

Mistral ships 1T-param weights under a permissive license that opens practical options few comparable models match. The FP8 and NVFP4 checkpoints give teams real flexibility based on available hardware: precision-critical data-center work takes FP8, and memory-constrained multi-GPU setups run NVFP4. The 1-million-token context and native multimodal support address concrete workflow patterns in code, long-document analysis, and security tooling. Teams getting the most out of this model will benchmark against their actual task distributions rather than headline numbers, and will read the license once before the first production commit rather than once after a problem surfaces.

| Mistral Large 4 Deployment Options |  |  | 
|---|---|---|
| feature | Open Weights | la Plateforme API | 
|---|---|---|
| access | Download from Hugging Face | Managed API | 
| checkpoints | FP8 and NVFP4 | N/A | 
| input cost | free (self-host) | $0.70 per million tokens | 
| output cost | free (self-host) | $2.10 per million tokens | 
| cached input | free (self-host) | $0.07 per million tokens | 
| commercial use | yes (license applies) | ✓ | 

- Q: What is Mistral Large 4, also known as Le Chonk?
- Mistral Large 4 is a natively multimodal open-weight language model released by Mistral AI in October 2026 with 1 trillion total parameters. Its Mixture-of-Experts design activates only a fraction of those parameters per token. The Le Chonk nickname reflects its scale. It handles text, images, code, and long documents within a 1-million-token context window per Hugging Face (2026).
- Q: How many active parameters does Mistral Large 4 use per inference step?
- Per Mistral AI (2026), 52 billion parameters are active per token. The Hugging Face model card puts the figure at approximately 49 billion active parameters. Both agree that roughly 5 percent of the full 1 trillion total parameters fire during any single inference step, keeping compute costs well below those of a comparably sized dense model.
- Q: Is Mistral Large 4 truly open source or just open weight?
- It is open weight, not open source. The trained weights are freely downloadable and commercially usable under the accompanying license, but Mistral has not disclosed the training dataset or internal training tooling. Before shipping a product that redistributes the weights or deploys them in high-risk domains, check the specific clauses in the license text posted on the model card.
- Q: What hardware is needed to run Mistral Large 4?
- Requirements vary by checkpoint. NVFP4 uses approximately 25 percent of the memory a full FP16 model would need, making multi-GPU consumer setups feasible. FP8 targets data-center class hardware such as H100 or A100 configurations, consuming roughly 50 percent of FP16 memory. KV-cache memory adds to weight memory at long context lengths; consult the official Hugging Face model card for exact GPU configuration guidance.
- Q: What workloads is Mistral Large 4 best suited for?
- Primary applications include multilingual text generation, code writing and review, structured data extraction, image understanding, and long-document analysis. TechCrunch reported that Mistral specifically highlights cybersecurity workflows, particularly log analysis and threat investigation, as strong fits given the model’s reasoning depth and 1-million-token context capacity.
