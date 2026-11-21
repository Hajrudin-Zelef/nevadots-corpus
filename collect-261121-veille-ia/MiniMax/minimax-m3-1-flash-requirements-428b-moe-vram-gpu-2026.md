---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-requirements-428b-moe-vram-gpu-2026
title: "MiniMax M3.1 Flash Requirements: 428B MoE, VRAM & GPU (2026)"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "flash", "memory", "parameters", "official", "requirements", "weights", "about"]
source: https://www.oflight.co.jp/en/columns/minimax-m3-1-flash-requirements-vram-2026
source_anchor: ""
source_lines: [1, 194]
section: "MiniMax"
---
MiniMax M3.1 Flash (428B MoE, ~23B active): ~250GB weights at 4-bit, 260-300GB RAM. Needs a 512GB Mac or 4 GPUs. Sizes, KV cache, API cost. Updated Oct 2026

**MiniMax M3.1 Flash is a multimodal MoE model from MiniMax with 428B total and about 23B active parameters. At 4-bit (NVFP4) the weights are about 250GB, and you should plan for roughly 260-300GB of memory including the KV cache.** It will not run on a single 24GB or 32GB GPU or on a 64GB Mac. The realistic floor is a Mac Studio (M3 Ultra) with 512GB of unified memory or a server with four 96GB-class GPUs. If you do not need to run it locally, the official API is cheap at $0.15 per 1M input tokens and $0.60 per 1M output tokens, which is enough for personal trials.

MiniMax M3.1 Flash launched as a hosted service on October 19, 2026, and its weights were published on October 23 on Hugging Face (MiniMaxAI/MiniMax-M3.1-Flash) and ModelScope, with day-0 vLLM support. It had been teased under the codename "Space Bunny". This article works out the requirements from the published file sizes and official figures, and labels anything MiniMax has not stated as our own estimate. Most benchmark numbers are vendor-reported (as of October 24, 2026).

## Requirements at a glance

| Precision / quantization | Weight size | Memory needed (guide) | Example setup | 
|---|---|---|---|
| BF16 (reference, not distributed) | ~856GB (estimate) | 900GB+ | 8x H200 (1,128GB) class; unrealistic for individuals | 
| FP8 | 430GB+ (estimate) | ~450-500GB | 8x H100 80GB (640GB), 512GB Mac (short context) | 
| Official NVFP4 (4-bit) | 252.4GB (repo, incl. 2.3GB DSpark drafter) | ~260-300GB | 512GB Mac, 4x RTX PRO 6000 (96GB), 4x B300 | 
| ~3-bit GGUF (estimate) | ~170-190GB | ~190-220GB | 256GB Mac (short context), 3x 96GB GPUs | 
| ~2-bit GGUF (estimate) | ~125-140GB | ~140-170GB | 192GB Mac or workstation with 192GB-class memory | 

If you want to check quickly whether your GPU or Mac can run it, our VRAM calculator (free, no sign-up) estimates the required VRAM from the model, quantization and context length you pick. MiniMax M3.1 Flash is available in its model list.

In the table, 252.4GB (repo) and 250.1GB (the NVFP4 checkpoint for vLLM) are official figures. BF16, FP8, 3-bit and 2-bit are **our estimates** derived from 428B parameters (BF16 = 2 bytes, FP8 = 1 byte, ~3.5 bits for 3-bit class, ~2.7 bits for 2-bit class). "Memory needed" adds KV cache and runtime headroom on top and is not an official number. MiniMax has not published requirements for consumer hardware.

## Model overview

| Item | Detail | 
|---|---|
| Total parameters | 428B (official) | 
| Active parameters | ~23B per token (MoE) | 
| Input | Text, image, video (native multimodal) | 
| Output | Text | 
| Context length | Up to 1,000,000 tokens | 
| Max output | 524,288 tokens | 
| Attention | MiniMax Sparse Attention (MSA) | 
| Decoding speed-up | DSpark speculative decoding (2.3GB draft head included) | 
| License | MiniMax Community License | 
| Weights | Hugging Face, ModelScope | 

With about 23B active parameters, per-token compute is closer to a 20B-class model. But the experts selected change with every token, so all 428B of weights must sit in memory. "23B active" describes speed, not memory: required memory stays at the 428B level.

## Why Hugging Face shows 220B while the official figure is 428B

The safetensors metadata on Hugging Face shows roughly 220B parameters, about half of the official 428B. This is most likely because the 4-bit (NVFP4) weights are stored packed two per 8-bit integer (U8), and the viewer counts the packed elements as parameters. The file size (about 252GB) is consistent with 428B parameters stored at around 4 bits (about 214GB, plus scales and layers kept at higher precision). Use 428B, not 220B, for memory estimates.

The same kind of mismatch shows up elsewhere; our DeepSeek V4.1 Flash requirements article covers a similar disagreement in total parameter counts. Always check the actual file sizes in the repository rather than trusting the model page's headline number.

## NVFP4 and quantization-aware training (QAT)

M3.1 Flash was not squeezed to 4-bit after the fact. It was trained with quantization-aware training (QAT), so the model learns to work at low precision. According to the published details, the routed experts and the sparse-attention KV path were trained for NVFP4, shared experts for MXFP8, and the query path in FP8. Only the parts that tolerate low precision are 4-bit; the sensitive parts stay at 8-bit.

As a result, the NVFP4 checkpoint for vLLM is 250.1GB versus 451.5GB for the previous MiniMax M3 (MXFP8), a 44.6% reduction. Because the official release is already 4-bit, going smaller means relying on community GGUF builds. About 25 community quantizations are listed on Hugging Face, but their quality has not been well verified yet.

## KV cache and the 1M-token context

The KV cache matters most for long contexts. Officially, M3.1 Flash uses 34,560 bytes per token, 43.75% less than the 61,440 bytes of M3. Our calculation from those figures:

| Context length | KV cache (M3.1 Flash) | For reference: M3 (61,440 B/token) | 
|---|---|---|
| 32K (32,000 tokens) | ~1.1GB | ~2.0GB | 
| 128K (128,000 tokens) | ~4.4GB | ~7.9GB | 
| 256K (256,000 tokens) | ~8.8GB | ~15.7GB | 
| 1M (1,000,000 tokens) | ~34.6GB | ~61.4GB | 

These figures are for a single request and scale with concurrency. The reported result of 76 concurrent 256K-token sequences on 4x B300 (versus 28 for M3) comes from both the compressed weights and the smaller KV cache. For a single user, 256K adds only about 9GB on top of the 250GB of weights, and even 1M adds about 35GB. Be aware that image and video inputs can inflate the token count quickly.

## Running on Apple Silicon (Mac Studio M3 Ultra)

Unified-memory Macs can hold huge MoE models without a GPU memory wall. The 4-bit M3.1 Flash is about 250GB, so a 512GB Mac Studio (M3 Ultra) fits the weights, KV cache and OS with room to spare. A 256GB Mac is almost full with the 4-bit weights alone, leaving little for macOS and the KV cache, so a ~3-bit build (about 170-190GB) with a short context is the realistic choice. Macs with 128GB or less struggle even at 2-bit.

Note that the Mac tooling is not ready right after release. The officially recommended engines are SGLang, vLLM and TokenSpeed, mostly aimed at NVIDIA and AMD servers. Transformers, KTransformers and unsloth support is listed as in progress, and llama.cpp is not officially recommended. Running on Mac via MLX or GGUF means validating community quantizations yourself.

## NVIDIA multi-GPU configurations

- 4x B300 (the configuration in the official report): 76 concurrent 256K sequences. Server-class, not something individuals buy

- 4x RTX PRO 6000 (96GB) = 384GB: fits 4-bit (about 250GB) with roughly 130GB left for KV cache. Workstation-class, and one of the few NVIDIA setups here within reach of individuals or small teams

- 8x H100 80GB = 640GB: a conventional server that can even hold FP8 (430GB+)

- 8x H200 = 1,128GB: for running BF16 (about 856GB)

- A single RTX 5090 (32GB) or 4090 (24GB): far too small. Even 8x 24GB = 192GB cannot hold the 4-bit weights

When combining multiple GPUs over PCIe, inter-GPU traffic is slow and MoE expert distribution can hurt throughput. Server GPUs with NVLink have an advantage at the same memory capacity. The fact that MiniMax also reports GB300, Rubin and AMD MI355X support reflects that data-center hardware is the main target.

## Offloading to CPU and RAM

When GPU memory is short, you can keep expert weights in system RAM and compute only the active ones on the GPU. This is what KTransformers-style engines do, and a workstation with 256-512GB of DDR5 plus one 24-48GB GPU is the imagined setup. However, KTransformers support for M3.1 Flash is still in progress, so it is not usable yet. Even once it works, speed is bound by RAM bandwidth and will be much slower than on server GPUs.

## Recommended setups by budget

| Budget | Setup | Realistic use | 
|---|---|---|
| One 24-32GB GPU | Does not fit | Use the API, or pick a 40B-class model such as Recursive Flint for local use | 
| 64-128GB Mac | Tight even at 2-bit | Use the API | 
| 256GB Mac | ~3-bit GGUF, short context | Experiments; check the quality loss yourself | 
| 512GB Mac Studio (M3 Ultra) | 4-bit (~250GB) with generous context | Personal daily use; tooling still maturing | 
| 4x RTX PRO 6000 (96GB) | 4-bit (vLLM / SGLang) | Small-team in-house server | 
| 8x H100 / 4x B300 | FP8 to 4-bit, many concurrent users | Production and multi-user workloads | 

## API versus self-hosting cost

The official API costs $0.15 per 1M input tokens, $0.60 per 1M output tokens and $0.015 per 1M cached input tokens, with no long-context surcharge. Prices vary by provider: OpenRouter reportedly lists $0.10 and $0.50, and Together AI offers a serverless endpoint. For example, 100M input and 20M output tokens a month comes to $0.15 x 100 + $0.60 x 20 = $27.

One catch: M3.1 Flash cannot turn reasoning off. The chat template lets you choose an effort level of low, medium, high, xhigh or max, but if you leave it unset it defaults to max. Reasoning tokens are billed as output on the API, so even short questions can produce far more output than expected. Try low first and check the cost.

Once you account for hardware, power and operations, self-hosting will realistically never break even against API usage on the order of $27 a month. Running locally makes sense only when sensitive data cannot leave your network, when you process large volumes of tokens continuously, or when you want to inspect or modify the model.

## License caveats

M3.1 Flash is released under the MiniMax Community License. Commercial use is allowed, but you must show the attribution "Built with MiniMax M3". If annual revenue is US$20M or less, a one-time email notice is required; above US$20M, prior written authorization is required. It is not an OSI-approved open-source license, and differs sharply from DeepSeek V4.1 Flash, released around the same time under the MIT license. Read the full LICENSE on the distribution page before embedding it in a commercial product.

## How to run it (vLLM, SGLang, Docker)

- vLLM: `vllm serve MiniMaxAI/MiniMax-M3.1-Flash` starts an OpenAI-compatible server; supported since day 0

- SGLang: use the Docker image `lmsysorg/sglang:latest` and start it with `--shm-size 32g`

- TokenSpeed: one of the officially recommended engines

- Docker Model Runner: `docker model run hf.co/MiniMaxAI/MiniMax-M3.1-Flash`

- Transformers, KTransformers, unsloth: integrations in progress

- llama.cpp: not officially recommended; community GGUFs are at your own risk

Recommended sampling is temperature 1.0 and top_p 0.95. The reasoning effort is set through the chat template. The 2.3GB DSpark draft head ships in the repository and can speed up decoding on engines that support it. For multi-GPU runs, match the engine's tensor-parallel setting to your GPU count.

## Benchmarks and cautions

| Benchmark | Score | Note | 
|---|---|---|
| DeepSWE v1.1 | 73.7 | Vendor-reported | 
| OSWorld 2.0 | 61.7 | Vendor-reported | 
| Terminal Bench 4.0 | 38.1 | Vendor-reported | 
| Automation-Bench | 62.8 | Vendor-reported | 
| BankerToolBench | 76.2 | Vendor-reported | 
| CyberGym | 82.2 | Vendor-reported | 
| HLE (no tools) | 43.4 | Vendor-reported | 
| Vals Index | 53.78% | Third party | 
| AA-Omniscience hallucination rate | 93% | Third party (Artificial Analysis); a caution | 

The vendor-reported scores are self-reported and have seen little independent reproduction. Artificial Analysis reports a 93% hallucination rate on AA-Omniscience, a metric for how often a model answers incorrectly instead of admitting it does not know. A high number suggests it tends not to say "I don't know". For fact-sensitive uses, pair it with search or internal documents and verify the output.

## Comparison with other models

| Model | Scale | Weights (guide) | License | Entry point | 
|---|---|---|---|---|
| MiniMax M3.1 Flash | 428B / ~23B active | ~250GB (NVFP4) | MiniMax Community License | 512GB Mac, 4x 96GB GPUs | 
| MiniMax M3 | Previous generation | 451.5GB (MXFP8) | Not confirmed | 8x B200 (official reference) | 
| DeepSeek V4.1 Flash | 763B (as shown; 552B in some sources) | ~510GB (official) | MIT | 8x H200, 512GB-class Mac (~430GB at 4-bit) | 
| Kimi K3 | 2.8T MoE | ~1.4TB (MXFP4) | Reported as modified MIT | Large cluster | 
| Recursive Flint | 40B / ~2.5B active | ~23-26GB at 4-bit | Apache 2.0 | 24-32GB GPU, 48GB Mac | 

Among large open-weight MoE models, M3.1 Flash has about half the weights of DeepSeek V4.1 Flash and a fraction of Kimi K3, putting it near the upper edge of what an individual can reach. See also our articles on DeepSeek V4.1 Flash requirements, self-hosting Kimi K3 open weights, and the older MiniMax M2.5. If you want something that runs on a single GPU, a small model like the one in Recursive Flint requirements is the practical alternative.

## Troubleshooting

- Out of memory (OOM): lower the context length and concurrent requests, reduce `--max-model-len`, then add GPUs. The KV cache reaches about 35GB at 1M tokens

- Model fails to load: your vLLM or SGLang version may be too old; day-0 support is in the latest releases, so update first

- Shared memory errors in Docker: SGLang needs `--shm-size 32g`

- Slow or very long outputs: reasoning effort defaulted to max because it was unset; lower it to low or medium

- Quality drop: community 2-3 bit quantizations are poorly verified; compare against the official NVFP4

- Memory spikes with images or video: more input tokens mean a larger KV cache; reduce resolution or frame count

## FAQ

How much memory does MiniMax M3.1 Flash need?

The official NVFP4 (4-bit) weights are about 252GB; with KV cache and runtime headroom, plan for 260-300GB. Our estimates are about 190-220GB at ~3-bit and 140-170GB at ~2-bit, but those are our own calculations, not official figures.

Does it run on an RTX 4090 or 5090?

Not on a single card. The weights alone are about 250GB versus 24GB or 32GB of VRAM, and even 8x 24GB = 192GB cannot hold the 4-bit weights. For local use, pick a smaller model; otherwise use the API.

Can I run it on a Mac Studio?

A 512GB M3 Ultra Mac Studio can hold the 4-bit weights (about 250GB) and the KV cache. With 256GB you are limited to ~3-bit or lower and short contexts. Right after release, Mac tooling is immature, so you must validate community quantizations yourself.

Why does Hugging Face show 220B?

The official total is 428B. The 4-bit (NVFP4) weights are stored two per 8-bit integer, and the viewer appears to count those packed elements, showing about half. Use 428B for memory estimates.

Can I use it commercially?

Yes, under the MiniMax Community License, with the attribution "Built with MiniMax M3". Companies with annual revenue of US$20M or less must send a one-time email notice; above that, prior written authorization is required. It is not as permissive as MIT, so read the full LICENSE.

株式会社オブライト

Oflight Inc. is a Tokyo-based IT company providing software development, digital signage, web development, network systems, and IoT/AR/VR technology services.

Related free tools (no sign-up, instant results)
