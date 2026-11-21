---
id: collect-261121-veille-ia/nvidia-hardware/nemotron-vram-requirements-local-deployment-nano-super-ultra-gpu-guide-nexgpu
title: "Nemotron VRAM Requirements & Local Deployment: Nano, Super, Ultra GPU Guide | NexGPU"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "b-a3b", "vram", "nano", "layers", "bf16", "nvfp4", "card"]
source: https://nexgpu.net/en/models/nemotron/
source_anchor: ""
source_lines: [1, 81]
section: "NVIDIA_Hardware"
---
Text LLM Â· Hybrid Mamba-MoE

# Self-host Nemotron: from 30B-A3B on one card to 550B across eight

NVIDIA's own open-weight family now runs from Nemotron 3 through 3.5 Lightning. This page gives you the real VRAM footprint of every variant, the card that actually fits it, and the handful of traps everyone hits when self-hosting.

Nemotron Â· self-hosted

First, clear up a common mix-up: today's Nemotron is not the 2024 Llama-finetune line. The current mainline is the Nemotron 3 family â Nano 30B-A3B, Super 120B-A12B, Ultra 550B-A55B â plus Nemotron 3.5 Lightning 30B-A3B, released August 2026. The backbone interleaves Mamba-2 state-space layers, Transformer attention and MoE. Nano is the clearest example: of 52 layers, 23 are Mamba-2, 23 are MoE, and only 6 are true attention layers, with each MoE layer carrying 128 routed experts plus 1 shared expert and activating 6 per token. This is not the dense Transformer you are used to, and it does not deploy like one.

The single biggest way people get burned self-hosting Nemotron is the MoE naming. The A3B in 30B-A3B means roughly 3B parameters activate per token, so compute is cheap and generation is fast â but VRAM is billed at the full 30B. All the weights have to be resident; sparse activation saves FLOPs, not memory. The real numbers: Nemotron 3 Nano 30B-A3B is ~60GB in BF16, ~36GB at Q8, ~21GB at Q4_K_M. The official 3.5 Lightning NVFP4 checkpoint repo is roughly 21.6GB, and the community GGUF UD-Q4-K-XL lands around 20GB. Size hardware as if this were a 3B model and you will OOM before the first token.

The other underrated factor is context. Nemotron 3 supports up to 1M tokens natively, but Nano ships with a 256K default, and the model card says why: VRAM. In a hybrid architecture the Mamba layers maintain an SSM state cache while the attention layers maintain a KV cache, and both grow with concurrency and sequence length. "The weights fit" and "the service runs" are two different claims. That is why every tier below leaves headroom. On NexGPU an RTX 5090 32GB is $0.723/GPU-hour and an A100 PCIE 80GB is $0.824/GPU-hour, billed per second â one hour of testing is far cheaper than buying the wrong card.

01 â

## Current Nemotron variants and what they actually cost in VRAM

Official checkpoints live under the nvidia org on Hugging Face; code and deployment cookbooks at github.com/NVIDIA-NeMo/Nemotron

| Version | Parameters | VRAM | Context | Notes | 
|---|---|---|---|---|
| NVIDIA-Nemotron-3.5-Lightning-30B-A3B | 30B-A3B (30B total / 3B active) | NVFP4 ~21.6GB Â· GGUF UD-Q4-K-XL ~20GB Â· Q8 ~33GB Â· BF16 ~60GB | 1M | Released August 2026 and tuned specifically for the execution layer of always-on agents â tool calls, result validation, high-volume routine operations. NVIDIA claims up to 4x the output speed of similarly sized models and 86% on PinchBench. Ships with MTP plus dedicated DFlash and DSpark draft models for speculative decoding. Licensed under OpenMDW-1.1 with weights, training data and recipes all released â the most permissive tier in the family. The NVFP4 build fits a single RTX 5090. | 
| NVIDIA-Nemotron-3-Nano-30B-A3B (BF16/FP8/NVFP4) | 30B-A3B (~31.6B total / ~3.5B active) | BF16 ~60GB Â· Q8 ~36GB Â· Q4_K_M ~21GB | 1M native, 256K in the default config | The December 2025 reasoning baseline, and the best-documented member of the family with the widest community quant coverage. Reasoning is on by default; turn it off with enable_thinking=False in the chat template, and cap chain-of-thought length with reasoning_budget. Supported by transformers 5.3.0+, vLLM 0.12.0+, TensorRT-LLM, SGLang and llama.cpp. Six languages: English, Spanish, French, German, Italian, Japanese. | 
| Nemotron-3-Nano-Omni-30B-A3B-Reasoning | 30B-A3B multimodal (~31B total / ~3B active) | NVFP4 ~18-22GB Â· FP8 ~35-40GB Â· BF16 60GB+ before multimodal inputs and KV cache | 1M | Released April 2026, unifying video, audio, image and text on one backbone for enterprise Q&A, summarisation, transcription and document intelligence. The catch: multimodal tokens add on top of the text baseline. 24GB only covers short-context text-only experimentation; budget 40GB or more for real document, video or audio workloads. | 
| NVIDIA-Nemotron-3-Super-120B-A12B-FP8 | 120B-A12B (120B total / 12B active) | FP8 needs a minimum of 2x H100 80GB; fits a single B200/B300 with adjusted tensor parallelism | 1M | The March 2026 production workhorse, a Latent MoE variant that NVIDIA benchmarks at 5x the throughput of the previous Super and 85.6% on PinchBench. MTP speculative decoding is built in and reasoning mode is switchable. NVIDIA recommends temperature=1.0 and top_p=0.95 across all tasks. Seven languages including Chinese, which Nano does not cover. Licensed under the NVIDIA Nemotron Open Model License. | 
| NVIDIA-Nemotron-3-Ultra-550B-A55B (BF16/NVFP4) | 550B-A55B (550B total / 55B active) | NVFP4 checkpoint 275GB -> 8x H100 80GB or 4x B200; BF16 ~1,100GB -> 8x H200 or 16x H100 | 1M | Released June 2026, scoring 48 on the Artificial Analysis Intelligence Index â the leading US open-weights model. Day-0 vLLM support. Watch one trap: 8x H200 totals 1,128GB, so after 1,100GB of BF16 weights you have roughly 28GB left across the whole node for KV cache, which will not sustain real batch sizes. For a single node, use NVFP4. | 

02 â

## Pick the card by workload

Budget VRAM as weights plus SSM state cache plus KV cache â every tier below leaves headroom

- NVFP4/Q4 builds of 3.5 Lightning or Nano for single-card evaluation and agent prototyping RTX 5090 32GB$0.723/GPU-hour Weights are 20-21.6GB, so 32GB leaves a comfortable ten-plus GB for hybrid caches and concurrency; a 24GB card runs out the moment you open the context.
- Nano Omni multimodal serving at 4-bit/FP8, or long-context Nano on one card RTX A6000 48GB$0.817/GPU-hour 48GB holds the 4-bit multimodal weights plus the cache that video and audio tokens generate, and costs less than an A100 80GB.
- Nemotron 3 Nano or 3.5 Lightning at full BF16 for fine-tuning baselines or quality comparison A100 PCIE 80GB$0.824/GPU-hour BF16 weights are around 60GB, so a single 80GB card fits them with enough room left for long context â the cheapest whole-card full-precision option.
- Nemotron 3 Super 120B-A12B FP8 in production, serving real inference traffic H100 SXM 80GB x2$3.582/GPU-hour 2x H100 80GB is NVIDIA's stated minimum, and SXM interconnect bandwidth keeps tensor-parallel-size 2 from being throttled by communication.

03 â

## Four steps to a running Nemotron

Using 3.5 Lightning NVFP4 on one card; Super and Ultra differ only in parallelism

1. 01### Launch an instance with vLLMPick a card from the table above in the NexGPU console and boot a prebuilt vLLM image so you skip installing CUDA and compiling Mamba kernels. Version matters: Nemotron 3 Nano needs vLLM 0.12.0+, Super's hybrid SSM kernel needs 0.17.1+, and NVIDIA's Ultra recipe uses the v0.22.0 image. If the version is too old, adding --trust-remote-code will not save you â the compiled kernel is simply missing. Confirm over SSH or the web terminal first. `nvidia-smi && python -c "import vllm; print(vllm.__version__)"`
2. 02### Pull the weightsFetch the precision you want from the nvidia org on Hugging Face. NVFP4 is around 21.6GB, BF16 around 60GB â mind where they land. NexGPU storage is $0.414/GB-month at the median, and compute billing stops when the instance stops while storage keeps billing until you destroy it, so clean up checkpoints you are done with. `hf download nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4 --local-dir /workspace/lightning`
3. 03### Serve it, and always pass the SSM cache flagThis is the classic Nemotron self-hosting trap. In a hybrid architecture the Mamba-2 state cache is a separate thing from the KV cache, and the SSM state cache should be float32 regardless of checkpoint precision, or output quality degrades in ways that are hard to trace. The released Nemotron 3 Super checkpoints bake this in; everywhere else, set it explicitly. If you hit CUDA OOM at startup, lower --max-num-seqs before --max-model-len â the model card says cut concurrency first. And do not enable Mamba prefix caching together with MTP speculative decoding: on NemotronH-style architectures that combination crashes during cudagraph profiling. `vllm serve /workspace/lightning --mamba-ssm-cache-dtype float32 --max-model-len 131072 --max-num-seqs 16 --port 8000`
4. 04### Verify, and switch off thinking when you do not need itNemotron 3 has reasoning on by default, which you usually want off for tool calling and high-throughput execution â use enable_thinking=False in the chat template. Sampling follows NVIDIA's guidance: temperature=0.6 and top_p=0.95 in thinking mode, down to temperature=0.2 for pure instruction execution, while Super recommends temperature=1.0 with top_p=0.95 across the board. The endpoint is OpenAI-compatible, so most frontends and agent frameworks only need a new base_url. `curl http://localhost:8000/v1/chat/completions -H "Content-Type: application/json" -d '{"model":"/workspace/lightning","messages":[{"role":"user","content":"Explain the A3B in an MoE name in one sentence"}],"temperature":0.6,"top_p":0.95,"chat_template_kwargs":{"enable_thinking":false}}'`

## What running Nemotron actually costs

Start with the common case: 3.5 Lightning NVFP4 on a single RTX 5090 32GB at $0.723/GPU-hour, so an overnight 8-hour agent regression run is 0.723 x 8 = $5.784. Want a BF16 quality comparison? An A100 PCIE 80GB at $0.824/GPU-hour running a full day is 0.824 x 24 = $19.776. For production you move to Super: two H100 SXM 80GB is 3.582 x 2 = $7.164/hour, and an 8-hour working day is 7.164 x 8 = $57.312 â orders of magnitude below the entry cost of buying two H100s. If you want to touch the Ultra 550B ceiling, the 275GB NVFP4 checkpoint needs 8x H100 SXM at 3.582 x 8 = $28.656/hour, so a two-hour capability evaluation is $57.312 â the same price as that Super working day. Storage is separate: 60GB of BF16 weights at the median rate is 60 x 0.414 = $24.84/month, while the 22GB NVFP4 build is 22 x 0.414 = $9.108/month. Storage keeps billing after the instance stops, so destroy what you no longer need. Everything is metered per second and priced per hour, with no minimum, no setup fee and no quota request.

04 â

## FAQ

## ### Nemotron 3 Nano is 30B-A3B â can I size my GPU as if it were a 3B model?


## ### vLLM says the kernel is missing, or output quality looks wrong. How do I debug it?


## ### How many GPUs does Nemotron 3 Ultra 550B actually need?


## ### Can I use Nemotron commercially under its licence?


## ### Should I pick Nemotron 3 Nano or 3.5 Lightning?


## ### Is the Nemotron-4 340B I found online the same as the Nemotron 4 people are talking about now?


## Start building on NexGPU

Enterprise R&D team or solo developer â either way, your first job can be running in minutes.

Sign up to browse live network pricing. No payment method required.
