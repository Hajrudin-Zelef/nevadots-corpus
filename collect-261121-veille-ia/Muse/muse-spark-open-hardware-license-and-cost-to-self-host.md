---
id: collect-261121-veille-ia/muse/muse-spark-open-hardware-license-and-cost-to-self-host
title: "Muse Spark Open: Hardware, License and Cost to Self-Host"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["card", "nvfp4", "muse", "spark", "meta", "bf16", "model", "open"]
source: https://dev.to/akaranjkar08/muse-spark-open-hardware-license-and-cost-to-self-host-4gah
source_anchor: ""
source_lines: [1, 103]
section: "Muse"
---
**Almost nobody should self-host Muse Spark Open: the model card asks for at least 700 GB of VRAM even in NVFP4, so for anything under billions of tokens a month the hosted endpoint at $1.25 per million input tokens is the cheaper and simpler route.** Meta released the open-weight 816B-parameter mixture-of-experts model on 15 October 2026[1]. Only 42B parameters are active per token, which helps speed but does nothing for memory: every expert still has to sit in RAM. Below: the weight arithmetic with the real file sizes, a comparison of three ways to run it, what the license says, and how Meta's benchmark numbers stack up against the reported competition.

- 816B total parameters, 42B active, 1M-token context, BF16 and NVFP4 weights[2].
- The BF16 repo holds 1.64 TB of safetensors across 65 shards[3]; the NVFP4 repo is 533 GB across 127 shards[8]. The card lists 700 GB of VRAM for NVFP4 and 2 TB for BF16.
- Two M5 Ultra Mac Studios (up to 512 GB unified memory each) can hold the model in an MLX quant, per Meta[1]. Eight 80 GB H100s (640 GB) fall under the card's 700 GB line, and the NVFP4 build targets Blackwell GPUs anyway[8].
- OpenRouter listed it at $1.25 input and $4.25 output per million tokens on 20 October 2026[4].
- License: use, fine-tuning, distillation and redistribution are allowed. Selling it as a Model-as-a-Service (API access to outputs) once you and your affiliates pass $20 million in gross revenue in any consecutive 12 months needs a separate agreement with Meta[7].
- Meta's own table has it trailing Kimi K3, GLM 5.3 and Qwen3.8 on Terminal-Bench 2.1 and the Artificial Analysis Intelligence Index[5].

Muse Spark Open is the open-weight sibling of the earlier closed Muse Spark; our earlier Muse Spark post covers that one. This release is natively multimodal, built for coding and agentic work, and carries a 1M-token context window[1].

The model card gives the architecture: a mixture-of-experts with 256 experts and 8 activated per token, 62 layers, a hidden dimension of 8192, and a 1.8B-parameter perception encoder. The Hugging Face metadata reports 822B parameters, the headline figure of 816B comes from Meta, and the gap is probably the encoder and embeddings, though the card does not say so[2].

Weights ship in BF16 and NVFP4. Inference support listed on the card covers SGLang, vLLM, Hugging Face Transformers, TensorRT-LLM, Unsloth and llama.cpp, with MLX for the Mac Studio setup[2]. Meta lists API hosting on Baseten, Fireworks AI, Groq, Modal, Nebius, OpenRouter and Together AI, with weight downloads also offered through Amazon SageMaker AI, Google Cloud Model Garden, Hugging Face and Microsoft Foundry Models[1].

Weight memory is parameter count times bytes per parameter. It is the floor, not the total.

- **BF16:** 816 billion x 2 bytes = 1,632 GB, about 1.63 TB.
- **NVFP4:** 816 billion x 0.5 byte = 408 GB is the floor. The real NVFP4 repo (Muse-Spark-Open-816B-A42B-NVFP4) is 533 GB across 127 shards, because of the per-block scale factors and tensors kept at higher precision[8].

The arithmetic lines up with the repo. The safetensors shards total 1,644,333,393,864 bytes, or 1.64 TB (1.50 TiB), spread over 65 files: 62 shards of roughly 26 GB (two at about 29.6 GB), two of about 0.55 GB and one of about 3.8 GB[3]. The extra 12 GB or so over the 816B x 2-byte estimate is the roughly 6B parameters the headline figure leaves out (the 1.8B perception encoder and the two 1.66B embedding matrices); the Hugging Face metadata counts 822B parameters, almost all BF16 plus about 0.13B in F32[2].

The card then asks for more than the weights alone: at least 700 GB of VRAM for NVFP4 and 2 TB for BF16[2]. The NVFP4 checkpoint is 533 GB, so the card's 700 GB figure leaves roughly 167 GB for KV cache, activations and runtime overhead (the card does not break the number down). Push toward the 1M-token window and the cache grows with it. No KV-cache size per token is published in the model card or launch posts, so treat any long-context budget as a guess until measured.

One point that trips people up: 42B active parameters means each token touches about 5% of the weights, which makes decoding far cheaper per token than a dense 816B model. It does not let you load 42B worth of experts. The router can pick any expert on any token, so all of them stay resident.

| Option | Memory available | What fits | Cost basis | Makes sense when | 
|---|---|---|---|---|

| 2x Mac Studio (M5 Ultra, MLX) | Up to 512 GB each, 1,024 GB pooled[1] | A 3-bit or 4-bit MLX quant (Hugging Face points to the oQ3e and oQ4e community builds[8]). Not BF16 (2 TB). The NVFP4 checkpoint targets NVIDIA Blackwell, not Apple silicon. | Hardware purchase, then power. Meta publishes no tokens-per-second figure; Hugging Face's launch post reports 18 tokens/s for a 2-bit llama.cpp quant on a Mac Ultra. | Private local work, one or two users, data that cannot leave the building |

| 8x 80 GB H100-class server | 640 GB total | Neither build by the card numbers. NVFP4 needs 700 GB and Blackwell-class GPUs. | Hourly GPU rental or purchase | Not on its own. Step up to 141 GB H200-class parts (8x = 1,128 GB), the NVFP4 recipe Hugging Face validates for the full 1M-token context. |

| Hosted API | Provider's problem | The full model, up to the 1M window | Per token: $1.25 input and $4.25 output per million on OpenRouter[4] | Everything below sustained, high-volume traffic |

The H100 row is plain arithmetic: 8 x 80 GB = 640 GB, which is under the 700 GB the card lists. That does not mean it is impossible with aggressive tuning, only that the card does not claim it. A node of 141 GB accelerators clears the bar, and Hugging Face validates it: its Inference Endpoints recipe runs the NVFP4 build on 8 x H200 141 GB at the full 1,048,576-token context, or on 8 x RTX Pro 6000 98 GB at 131,072 tokens[8]. The vLLM reference for NVFP4 needs Blackwell-class GPUs, so an H100 box is out. Check the vLLM and SGLang docs for the current tested configuration before you rent anything.

On the Mac route, Meta calls it "the first model in its class to run on your desktop using two Apple M5 Ultra Mac Studios with the latest MLX framework" and says the two machines behave "like one supercomputer with a terabyte of unified memory"[1]. Read that as a capability claim, not a speed claim. Memory bandwidth on the chip is listed at 1.2 TB/s, which sets the ceiling for decoding. The only measured figure published is Hugging Face's 18 tokens/s for a 2-bit llama.cpp quant on a Mac Ultra[8], a lossy build, so it does not extrapolate to the MLX setup.

On 20 October 2026 the OpenRouter model list showed Muse Spark Open at $1.25 per million input tokens and $4.25 per million output tokens, with a context of about 1.05M tokens. Five providers were listed (Modal, Together, Nebius, Fireworks and BaseTen), all at the same price[4]. Providers reached directly rather than through OpenRouter (Groq, Baseten, Modal and the rest) may price differently; their own rate cards were not checked, so confirm before you commit. OpenRouter's model page also notes the model is not currently available through EU in-region routing[6].

A worked example with those listed prices: a coding agent that reads 10 million input tokens and writes 2 million output tokens in a month pays 10 x $1.25 + 2 x $4.25 = $12.50 + $8.50 = $21.00. Use the AI model cost calculator to plug in your own volumes, and the AI token counter to measure what your prompts really weigh before you estimate.

The break-even is a division you can do yourself: monthly hardware cost divided by your blended price per million tokens. At a 4:1 input-to-output mix, the listed prices blend to (4 x $1.25 + $4.25) / 5 = $1.85 per million tokens. Take a hypothetical $10,000 per month of dedicated GPU capacity. You would need to push about 5.4 billion tokens through it every month, with the cluster busy, just to match the API bill. That is the figure of $10,000 divided by $1.85, rounded. The $10,000 is an illustration, not a quote; substitute your real rental or amortised cost.

Two things push the number against you. Idle GPUs bill the same as busy ones, and you also carry the engineering time for upgrades, batching and failure handling. Two things push it for you: a hard data-residency requirement, and traffic so steady that the hardware never idles.

The weights ship under the Muse Model License, a version titled "Muse Spark Open" and dated 15 October 2026[7]. The clauses below are taken from the full LICENSE.md text (8,299 bytes[3])[7]. The weights are also gated: you must accept the license on Hugging Face and submit a form before you can download. Read the file itself before you build a product on this.

What the license allows: use, modification including fine-tuning, distillation, merging, publishing, sublicensing and selling copies, subject to conditions. Meta claims no ownership of outputs.

What it restricts:

- **Attribution.** You must prominently display "Built with Muse Spark Open" in the user interface of each product or service that uses the model or any Derivative, or in its documentation, website or settings menu. Derivatives include any model fine-tuned, distilled or otherwise improved using the weights or their outputs.
- **Model-as-a-Service threshold.** If you or your affiliates give third parties API-style access to the model (or a Derivative) where they receive its outputs, and your combined gross revenue passes $20 million in any consecutive 12 months, you must notify Meta and sign a separate written agreement before offering it. Internal use, research, any commercial activity that is not Model as a Service, and using it to power features of your own product are excepted.
- **Usage Policy.** It is incorporated into the license and prohibits illegal activity, harassment, unlicensed professional practice, malware and deceptive content.
- **Age.** Not intended for individuals under 18.
- **No warranty.** The software is provided "AS IS" and Meta disclaims liability for derivatives.

Meta describes the terms as limited commercial terms that let many developers use the model for free[1]. The practical read: a startup, an agency or an internal tool is comfortably inside the line. A company reselling hosted inference at scale is not. This is a custom license, not Apache 2.0 or MIT, so it will not pass a procurement checklist that only whitelists OSI-approved licenses.

Everything in this table comes from the comparison table on Meta's model card (Muse Spark Open at xhigh reasoning; rival scores as Meta lists them, with some self-reported per MarkTechPost)[2][5]. Treat all of it as vendor-reported until independent evaluations land.

| Benchmark | Muse Spark Open | Kimi K3 | GLM 5.3 | Qwen3.8 | 
|---|---|---|---|---|

| MCP-Atlas | 89.2 | 82.3 | 84.2 | 84.5 |

| Terminal-Bench 2.1 (Terminus 2) | 76.4 | 85.0 | 83.9 | 82.0 |

| DeepSWE 1.1 | 60 | 68 | 67 | 57 |

| Toolathlon Verified | 72.8 | 76.5 | 73.0 | 72.5 |

| HLE with tools | 61.0 | 56.0 | 62.5 | 56.2 |

| Artificial Analysis Intelligence Index | 39 | 44 | 45 | 40 |

The pattern: Muse Spark Open leads on MCP-Atlas, which measures tool use over the Model Context Protocol, and on HLE-Diamond without tools (26.1 against Kimi K3's 22.2) and ZeroBench with tools (50.0 against 41.0)[5]. It trails on the terminal and software-engineering tests that matter most for a coding agent. MarkTechPost's own conclusion is that the most-capable-US-open-weight claim holds while Chinese open models still lead overall on the intelligence index, and it flags that the headline claim comes from Meta[5].

The card also reports a GPQA Diamond score of 89.0, OSWorld Verified at 80.4 and Terminal-Bench 2.1 at 76.4[2]. On safety, the card lists FORTRESS adversarial scores (ARS 5.8, ORS 17.4, lower is better) and StrongREJECT v2 at 0.5[2].

Pick it over a closed API when you need weights you control: fine-tuning on private data, a fixed model version that cannot be deprecated under you, or a data-handling rule that forbids third-party inference. Pick it for tool-use-heavy agents, where the MCP-Atlas result is the strongest in the table.

Skip it when your workload is a terminal-driven coding agent and you can get a higher Terminal-Bench score elsewhere, or when your volume sits well under the billions-of-tokens break-even above. For the cost side of the closed-versus-open choice, our DeepSeek V4.1 Flash pricing guide works through the same per-token arithmetic for another open-weight model.

A sensible first step costs almost nothing: send your real prompts to the hosted endpoint, measure quality on your own tasks, and only then price hardware. Benchmark tables decide very little; your own eval set decides most of it.

The model card lists at least 700 GB of VRAM for the NVFP4 build and 2 TB for BF16[2]. The raw BF16 weights are 1.64 TB on disk[3]; the rest of the requirement is KV cache and runtime overhead.

No. Even at 4-bit the NVFP4 checkpoint is 533 GB on disk before any cache or overhead[8]. Meta's showcase hardware is two M5 Ultra Mac Studios with up to 512 GB of unified memory each[1]. Hugging Face's launch post lists llama.cpp GGUF quants (Q2_K, Q4_K_S and Q8 tested on Apple Silicon), but even a 2-bit file of an 816B model needs hundreds of gigabytes[8].

Mostly. Per the license, commercial use is allowed with attribution, and the main restriction is a separate agreement if you sell it as a Model-as-a-Service and you and your affiliates pass $20 million in gross revenue in any consecutive 12 months[7]. Read the full LICENSE.md before relying on this summary.

OpenRouter listed $1.25 per million input tokens and $4.25 per million output tokens on 20 October 2026[4]. Prices change, so recheck on the day.

No. The router can send any token to any expert, so all 816B parameters stay loaded. The 42B figure governs compute per token, not memory.

## 8. Hugging Face blog, Muse Spark Open launch post (NVFP4 repo, deployment recipes, GGUF and MLX quants)

*Originally published at wowhow.cloud*

## Top comments (0)
