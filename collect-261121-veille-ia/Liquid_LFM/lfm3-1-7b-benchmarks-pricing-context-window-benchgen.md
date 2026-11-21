---
id: collect-261121-veille-ia/liquid-lfm/lfm3-1-7b-benchmarks-pricing-context-window-benchgen
title: "LFM3-1.7B: Benchmarks, Pricing & Context Window — Benchgen"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-10-29"]
keywords: ["lfm3-1", "model", "card", "attention", "context", "liquid", "window", "agentic"]
source: https://benchgen.com/models/liquid-ai/lfm3-1-7b
source_anchor: ""
source_lines: [1, 82]
section: "Liquid_LFM"
---
**Quick answer:** LFM3-1.7B is Liquid AI's 1.73B-parameter text model built on a new "tri-hybrid" architecture (gated short convolutions + sliding-window attention + global attention), released October 2026 with a 256K-token context window. Post-trained for agentic harnesses (Hermes Agent, Pi, OpenClaw), it scores 71.4% on BFCL V4, 80.4% on ToolSandbox, and 96.7% on IFEval — Liquid reports it can power agentic applications that previously needed ~9B-parameter models. At 4-bit precision it needs about 1GB of memory, fitting on a phone or laptop.


**Where LFM3-1.7B leads**

**Where it lags**

**Best for:** local/edge agent deployments — tool calling, structured outputs, and domain-specific fine-tuning — where on-device privacy and near-zero marginal inference cost matter more than frontier coding or deep knowledge recall.

LFM3-1.7B is Liquid AI's newest small model for local agents, introducing a "tri-hybrid" architecture: 24 layers combining 13 gated short-convolution blocks, 5 sliding-window attention blocks, and 6 global attention blocks. It was pre-trained on 22T tokens across two stages (a general mix plus a mid-training phase adding scientific papers, repository-level code, math, and agentic/tool-call traces while extending context to 256K tokens), then post-trained through supervised fine-tuning, mixed reinforcement learning, multi-domain on-policy distillation from specialist RL teachers, and a final agentic-RL stage run inside real harnesses.

Liquid positions LFM3-1.7B explicitly as a successor that "can power agentic applications that used to need 9B parameters" — its own benchmark table shows it beating same-class and larger models (Qwen3.5-4B, Qwen3.5-9B) on most agentic, tool-use, and instruction-following evaluations while trailing on knowledge-heavy and long-context-retrieval tasks (GPQA Diamond, HLE, RULER). The model also supports "deferred tools" (tool search), letting a harness keep large tool catalogs out of the prompt until needed — a capability aimed squarely at production agent deployments with many available tools.

| Field | Value | 
|---|---|
| Organization | Liquid AI | 
| Parameters | 1.73B (dense) | 
| Context window | 262,144 tokens (256K) | 
| Architecture | Tri-hybrid — 24 layers: 13 gated short-convolution, 5 sliding-window attention, 6 global attention | 
| License | LFM Open License v1.0 (lfm1.0) | 
| Release date | October 2026 | 
| Modality | Text (16 languages) | 

Open weights, self-hostable — no per-token API pricing. Available on Hugging Face in BF16, FP8, NVFP4, GGUF (llama.cpp/Ollama/LM Studio), MLX (Apple Silicon), and ONNX formats. At 4-bit quantization, the model uses about 1GB of memory; at 1,024 concurrent requests it can produce 3.6 billion tokens/day on a single NVIDIA B300.

| Benchmark | Score | Source | Date | 
|---|---|---|---|
| BFCL V4 | 71.39 | LFM3-1.7B model card | 2026-10 | 
| ToolSandbox | 80.36 | LFM3-1.7B model card | 2026-10 | 
| MCP-Atlas | 32.57 | LFM3-1.7B model card | 2026-10 | 
| AutomationBench | 41.29 | LFM3-1.7B model card (public subset) | 2026-10 | 
| Claw-Eval average (EN) | 68.64 | LFM3-1.7B model card | 2026-10 | 
| SWE-bench Verified | 43.40 | LFM3-1.7B model card (OpenCode harness) | 2026-10 | 
| SWE-bench Multilingual | 23.00 | LFM3-1.7B model card (OpenCode harness) | 2026-10 | 
| Terminal-Bench 2.1 | 12.92 | LFM3-1.7B model card (OpenCode harness) | 2026-10 | 
| LiveCodeBench v6 | 73.46 | LFM3-1.7B model card | 2026-10 | 
| LiveCodeBench Pro | 31.74 | LFM3-1.7B model card (25Q2 subset) | 2026-10 | 
| SciCode | 28.47 | LFM3-1.7B model card | 2026-10 | 
| IFEval | 96.66 | LFM3-1.7B model card | 2026-10 | 
| IFBench | 73.03 | LFM3-1.7B model card | 2026-10 | 
| Multi-IF | 89.10 | LFM3-1.7B model card | 2026-10 | 
| GPQA Diamond | 63.37 | LFM3-1.7B model card | 2026-10 | 
| Humanity's Last Exam (no tools) | 7.17 | LFM3-1.7B model card | 2026-10 | 
| AA-Omniscience (public) | 0.50 | LFM3-1.7B model card | 2026-10 | 
| AA-LCR | 52.83 | LFM3-1.7B model card | 2026-10 | 
| WebArena-Verified | 38.42 | LFM3-1.7B model card | 2026-10 | 

*Scores above are reported by Liquid AI and shown for context. They are not Benchgen measurements. Figures depend on harness, tools, and effort settings — all models in Liquid's comparison table were served with vLLM in BF16 at full context length; see the source for full methodology.*

**Excluded from the table above (bucket-C, documented per skill procedure):**

`browsecomp*` entities don't document this exact subset's methodology, so left unmapped rather than guessed.`tau3-banking` entity; per standing policy, never silently narrow/widen scope. Excluded, same as the Nanbeige4.3-33B precedent.
| Model | Params | BFCL V4 | ToolSandbox | IFEval | 
|---|---|---|---|---|
| **LFM3-1.7B** | 1.73B | 71.4% | 80.4% | 96.7% | 
| LFM2.5-2.6B | 2.6B | 55.9% | 78.0% | 95.4% | 
| Qwen3.5-4B | 4.7B | 55.1% | 75.4% | 92.5% | 
| Qwen3.5-9B | 9.7B | 62.8% | 76.4% | 92.1% | 

Despite being Liquid AI's smallest LFM3-generation model, LFM3-1.7B leads every compared model — including the 9.7B Qwen3.5-9B — on BFCL V4, ToolSandbox, and IFEval, while remaining small enough to run locally on a phone or laptop; the clear trade-off for builders is weaker raw knowledge recall (GPQA Diamond, HLE) versus the larger Qwen3.5 models.

LFM3-1.7B's benchmark spread is heavily weighted toward tool calling, instruction following, and short-horizon agentic tasks (BFCL V4, ToolSandbox, MCP-Atlas, Claw-Eval) rather than deep knowledge or long-horizon coding — consistent with Liquid's explicit guidance that the model is "not recommended for agentic coding or complex multi-step reasoning due to its small size." A high public score on tool-use benchmarks indicates strong general tool-calling reliability, but builders deploying it for a specific production agent (e.g. a fixed set of internal APIs) should still validate accuracy on their own tool schemas and task distribution rather than relying on the aggregate benchmark numbers alone.

```
from transformers import pipeline
generator = pipeline(
    "text-generation",
    model="LiquidAI/LFM3-1.7B",
    trust_remote_code=True,
)
messages = [{"role": "user", "content": "What is C. elegans?"}]
output = generator(
    messages,
    do_sample=True,
    temperature=0.1,
    top_k=50,
    repetition_penalty=1.1,
    max_new_tokens=2048,
)
print(output[0]["generated_text"][-1]["content"])
```
*Specs and scores sourced from Liquid AI's official Hugging Face model card and release blog post (liquid.ai/blog/lfm3); third-party/comparison-model scores (LFM2.5-2.6B, MiniCPM5-2B, Gemma 4 E4B, Qwen3.5-4B/9B) as reported in the same launch benchmark table. Last updated 2026-10-29.*
