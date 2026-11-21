---
id: collect-261121-veille-ia/liquid-lfm/lfm3-is-a-family-of-six-open-weight-models-from-liquid-ai
title: "LFM3 Is a Family of Six Open-Weight Models From Liquid AI"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-10-27"]
keywords: ["liquid", "text", "lfm3", "model", "lfm3-1", "models", "attention", "audio"]
source: https://www.layer3labs.io/guides/lfm3-explained
source_anchor: ""
source_lines: [1, 116]
section: "Liquid_LFM"
---
# LFM3 Explained

LFM3-1.7B fits in about 1 gigabyte at 4-bit precision, and the weights are free to run for businesses under $10 million in annual revenue.

Liquid AI released LFM3 on October 27, 2026, introducing a family of six open-weight Liquid Foundation Models (LFMs). The six models are all around 2 billion parameters and share one backbone. They cover text, vision, audio, omni (vision, audio and language in one model) and decision tasks. Every checkpoint branches from a common base model trained across 22 trillion tokens.

Liquid AI reports that the text model, LFM3-1.7B, outperforms the earlier LFM2.5-2.6B with 35 percent fewer parameters. It also reports 3.6 billion tokens per day on one NVIDIA B300 graphics processing unit (GPU).

All six checkpoints are open weights on Hugging Face. Liquid AI also offers them in its Playground and through its application programming interface (API). Fine-tuning takes one GPU.

## What LFM3 Is and Which Model to Use

LFM3 is Liquid AI's collection of six open-weight models sized around 2 billion parameters that share a single tri-hybrid neural network backbone.

| Model | Takes in | Produces | Best first job | 
|---|---|---|---|
| LFM3-1.7B | Text | Text | Local text agents and tool calling | 
| LFM3-VL-2B | Text, images, video | Text | Visual and video inspection | 
| LFM3-Audio-2B | Text, audio | Text, speech | Voice conversations and transcription | 
| LFM3-Omni-2B | Text, images, audio, video | Text, speech | Combined visual and spoken tasks | 
| d1-2B | Text, images, video | Structured typed answers | Form extraction and classification | 
| d1-Omni-2B | Text, images, video, audio | Structured typed answers | Voice-driven routing and intake | 

The Best first job column is Layer3Labs' mapping of Liquid AI's published model descriptions, and the last section explains it.

According to the Liquid AI model library documentation, LFM3-1.7B supports a 256K context window, while LFM3-VL-2B, d1-2B, and d1-Omni-2B support 128K.

Start with LFM3-1.7B for local text agents that manage a calendar, triage an inbox or use search tools. Pick the vision, audio, omni, or decision checkpoints when your application takes images or video, spoken input, or needs one-pass probability answers over a form or ticket.

All model weights can be downloaded directly from the Liquid AI Hugging Face collection, including dedicated repositories for LFM3-1.7B, LFM3-VL-2B, LFM3-Audio-2B, LFM3-Omni-2B, d1-2B, and d1-Omni-2B.

## Architecture Changes in the Tri-Hybrid Backbone

The LFM3 backbone uses a 24-layer tri-hybrid architecture designed through automated hardware-in-the-loop search across 14 edge and server hardware setups. The network combines 13 gated short convolutions, 5 sliding-window attention (SWA) layers, and 6 global attention layers.

Liquid AI says swapping some convolutions for sliding-window attention gave the best quality and speed tradeoff. Those 5 layers look at a 256-token window. The other 6 attention layers see the full context.

The attention layers introduce head-wise gated attention, applying a sigmoid gate that scales the output of each individual attention head so an inactive head can switch off completely. Liquid AI says this can help suppress activation outliers that hurt quantization.

Liquid AI pre-trained LFM3-1.7B-Base on 22 trillion tokens. Compared with LFM2, the mix adds more scientific papers, repository-level code, a wider range of knowledge sources and considerably more math. A final 3.5 trillion-token mid-training stage uses curated data with more agentic traces and tool calls and extends the context window to 256K tokens. Post-training was conducted in four successive phases: supervised fine-tuning (SFT) across 1.75 trillion tokens, mixed reinforcement learning (RL), multi-domain on-policy distillation from six specialist teacher models, and agentic reinforcement learning.

Liquid AI expanded the hardware in its architecture search from central processing units in one phone and one laptop for LFM2 to 14 device and backend combinations for LFM3: phones, a single-board computer, laptops, Arm and x86 cloud processors, GPUs and the Qualcomm Hexagon Neural Processing Unit (NPU).

## Reported Benchmark Performance Across Modalities

Liquid AI published its own benchmark results comparing LFM3-1.7B with LFM2.5-2.6B and four other open models. Liquid AI reports LFM3-1.7B ahead of LFM2.5-2.6B and Qwen3.5 9B on the Berkeley Function-Calling Leaderboard (BFCL) V4, ToolSandbox and IFEval, while larger models still win on agentic coding and knowledge.

| Benchmark | LFM3-1.7B | LFM2.5-2.6B | Qwen3.5 9B | 
|---|---|---|---|
| LiveClaw | 45.60 | 14.70 | 35.77 | 
| BFCL V4 | 71.39 | 55.89 | 62.83 | 
| ToolSandbox | 80.36 | 78.04 | 76.41 | 
| SWE-bench Verified (OpenCode) | 43.40 | 18.00 | 51.20 | 
| IFEval | 96.66 | 95.39 | 92.08 | 
| GPQA Diamond | 63.37 | 57.78 | 80.40 | 
| RULER 128K | 65.56 | 1.15 | 87.11 | 
| Average | 49.37 | 31.55 | 47.24 | 

Liquid AI reports that LFM3-1.7B scores 71.39 on the Berkeley Function-Calling Leaderboard (BFCL) V4 and 80.36 on ToolSandbox, outpacing both LFM2.5-2.6B and the larger Qwen3.5 9B model. On the general instruction-following metric IFEval, LFM3-1.7B achieved 96.66 points.

Liquid AI's table puts Qwen3.5 9B ahead on agentic coding (SWE-bench Verified, 51.20 against 43.40), science questions (GPQA Diamond, 80.40 against 63.37) and long context (RULER 128K, 87.11 against 65.56). Liquid AI notes that larger models still win on agentic coding and knowledge, where size matters most.

For visual reasoning, Liquid AI reports that LFM3-VL-2B achieved a 71.4 average score across 62 computer vision benchmarks, matching the 71.7 average score of Gemma 4 12B despite containing less than one-sixth of the parameters. The model uses an in-house 164-million-parameter vision encoder that matches the accuracy of Qwen3.5-2B's visual encoder while requiring 2.2 times fewer floating-point operations (FLOPs).

Liquid AI reports 73.1 on VoiceBench for LFM3-Audio-2B against 55.2 for LFM2.5-Audio-1.5B. Its speech tokenizer generates speech at about 87 times real time on one central processing unit (CPU) thread of an Apple M4 Max. The multimodal LFM3-Omni-2B model achieved 70.1 on Daily-Omni against 58.8 for Qwen2.5-Omni-3B. The decision model d1-2B reached 85.2 on Decision Bench, outscoring the previous d1-3B model while operating with one-third fewer parameters.

## LFM3-1.7B Fits in About 1 GB at 4-Bit Precision

At 4-bit precision, LFM3-1.7B takes about 1 gigabyte of memory. Liquid AI says that is small enough for any phone or laptop.

A full 128K-token context needs 1.6 gigabytes of key-value (KV) cache memory in llama.cpp at Q4_0. In comparison, Liquid AI reports that Qwen3.5-4B requires 4.3 gigabytes and MiniCPM5-2B requires 5.6 gigabytes for identical context lengths. Per parameter, LFM3-1.7B matches LFM2-1.2B on short prompts across edge devices, and with 8K tokens in context it is about 25 percent faster.

Liquid AI ran serving tests with llama.cpp at Q4_0 and a 32K-token context on three devices. It decoded 1.8 to 2.8 times faster than the next-fastest model. It also used the least memory. On a single NVIDIA B300 GPU running SGLang in bfloat16 (BF16) precision, it produced 3.6 billion tokens per day at 1,024 concurrent requests.

Liquid AI lists base and several quantized checkpoint formats for LFM3-1.7B, including MLX for Apple hardware. It runs with Hugging Face Transformers, llama.cpp (needs a build with LFM3 support), vLLM and SGLang.

## License Terms and Access Points

Liquid AI distributes LFM3 under the LFM Open License v1.0, which makes commercial use free below $10 million in annual revenue and requires a paid license from Liquid AI at $10 million or more. Developers can download model weights from Hugging Face, run queries in the Liquid AI Playground, or call hosted endpoints through the Liquid AI API. The LFM3 pricing guide has the cost details.

## Operational Limits and Workloads to Avoid

According to the official LFM3-1.7B model documentation, the model is not recommended for agentic coding or complex multi-step reasoning due to its small size.

For knowledge-heavy workloads, Liquid AI advises pairing the model with an external retrieval pipeline or web search. On the Humanity's Last Exam benchmark, LFM3-1.7B achieved a score of 7.17 without external tools, which improved to 13.90 when connected to live web search tools. On the public AA-Omniscience factual accuracy test, the model scored 7.07 points.

Reasoning is on by default in LFM3-1.7B. Setting enable_thinking=False gives direct answers.

Liquid AI ran every benchmark in this guide itself, and no independent party has replicated them as of October 27, 2026. The first LFM3 release has no model near the size of LFM2.5-24B-A2B or LFM2.5-8B-A1B.

## What LFM3 Changes for Teams Running Local Models

At Layer3Labs, we build custom agents and workflow automation for business operations. In our internal analysis of Liquid AI's published model descriptions, we mapped each LFM3 model to the first job it fits (the Best first job column in the table above).

LFM3-1.7B fits local agents that triage an inbox, manage a calendar and use tool search to find the right tool. LFM3-VL-2B fits image and video inspection, while LFM3-Audio-2B fits voice conversations and transcription. LFM3-Omni-2B serves mixed visual-spoken applications, and the d1-2B and d1-Omni-2B checkpoints provide calibrated probability scoring for single-pass form extraction, invoice analysis, and automated ticket classification.

Who should not pick LFM3: Teams whose hardest work is agentic coding or complex multi-step reasoning should pick a larger model. The LFM3-1.7B model card does not recommend it for either. Enterprises with $10 million or more in annual revenue cannot deploy LFM3 for free under the standard open-weights terms and need a paid license from Liquid AI.

What would change our answer: Independent evaluations that disagree with Liquid AI's published benchmark table, or a posted API price from Liquid AI for the LFM3 models.

Download LFM3-1.7B and run 20 of your own real prompts through a local runtime. Judge whether its accuracy is good enough for your agent before you commit. You can review license terms and self-hosting memory sizing in our LFM3 pricing guide and compare architectural trade-offs in our LFM3 vs LFM2.5 comparison.

## Frequently Asked Questions

- LFM3 is a family of six open-weight artificial intelligence models developed by Liquid AI, built around a shared 2-billion-parameter tri-hybrid architecture. The release includes checkpoints specialized for text generation, computer vision, speech audio, omni multimodal processing, and automated decision scoring.
- The weights are open, but the LFM Open License is not an Open Source Initiative (OSI) approved license, and it sets a revenue threshold. Businesses under $10 million in annual revenue use LFM3 free. At $10 million or more, a paid license from Liquid AI is required.
- LFM3-1.7B contains 1.73 billion parameters across 24 computational layers. At 4-bit quantization, the model takes about 1 gigabyte of memory.
- The context window for LFM3-1.7B is 256K tokens for combined inputs and outputs. For LFM3-VL-2B, d1-2B, and d1-Omni-2B, the supported context window is 128K tokens.
- Liquid AI says LFM3-1.7B takes about 1 gigabyte of memory at 4-bit precision and fits on any phone or laptop.
- LFM3 was developed and released by Liquid AI on October 27, 2026.

### Plan Your Local Model Architecture

Book a free workflow audit with Layer3Labs to map your local hardware capacity, benchmark agent response quality, and select the right open-weight models for your team.

Book a Free Audit
