---
id: collect-261121-veille-ia/deepseek/run-deepseek-v4-1-flash-locally-vllm-guide
title: "Run DeepSeek V4.1 Flash Locally: vLLM Guide"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-10-17"]
keywords: ["deepseek", "flash", "weights", "vllm", "vram", "locally", "hugging", "face"]
source: https://www.deepseekv4guide.org/guides/v4-1-flash-local-deployment
source_anchor: ""
source_lines: [1, 131]
section: "DeepSeek"
---
# Run DeepSeek V4.1 Flash Locally: VRAM, vLLM & Quantization

How to run DeepSeek V4.1 Flash locally: MIT weights, a ~511 GB checkpoint, a 614 GB VRAM floor, the official vLLM image, MXFP4/MXFP8 and GGUF.

Updated Oct 17, 2026: 1017 weights are on Hugging Face under MIT, and a community GGUF now fits a single 128 GB DGX Spark. See the 1017 local guide.

## What It Takes to Self-Host V4.1 Flash

DeepSeek published V4.1 Flash under the MIT license on Hugging Face, so the weights are free to download, modify and serve commercially<sup>[4]</sup>. Open weights are not the same as easy weights, though. The model is a 552B-parameter MoE backbone plus 196.6B parameters of Engram conditional memory, for roughly 763B total parameters, and the published checkpoint is about 511 GB on disk<sup>[4]</sup><sup>[5]</sup>.

That single number reframes the project. Running V4.1 Flash is not a laptop exercise; it is a small-cluster exercise. DeepSeek wrote the serving recipe for data-center accelerators, not consumer GPUs, because every expert must be resident in memory even though only 6 routed experts activate per token<sup>[4]</sup><sup>[5]</sup>.

This guide follows the official path end to end: the Hugging Face download, the real VRAM floor, the Docker image DeepSeek ships, the full vLLM command, the quantization scheme, DSpark speculative decoding, and the third-party GGUF option for teams without a cluster<sup>[4]</sup><sup>[5]</sup>.

## The 614 GB VRAM Minimum, Explained

The vLLM recipe lists vram_minimum_gb at 614, a figure that already includes a 1.2x headroom factor over the raw weight footprint. That is the floor for the MXFP4/MXFP8 distribution DeepSeek publishes, not a BF16 model, which would be far larger<sup>[5]</sup>.

DeepSeek's validated configuration runs on a single GB200 NVL4 tray with 768 GB of HBM, using tensor parallelism of four across the four Blackwell dies. An alternative reference rig is eight H200 GPUs totalling 1,128 GB, again with tensor parallelism<sup>[5]</sup>.

This is why the local-deployment story keeps tripping people up. A model marketed as Flash and built for cheap inference still needs hundreds of gigabytes resident in memory, because all experts must be addressable even when only a handful activate per token<sup>[4]</sup>.

| Component | Requirement | Notes | 
|---|---|---|
| Minimum VRAM | 614 GB | 1.2x headroom over weights[5] | 
| Reference rig A | GB200 NVL4 (768 GB) | TP4 across four Blackwell dies[5] | 
| Reference rig B | 8x H200 (1,128 GB) | More headroom[5] | 
| Checkpoint on disk | ~511 GB | SafeTensors shards[5] | 
| Routed + DSpark experts | 259.5 GiB | MXFP4[5] | 
| Engram memory | 188.8 GiB | Two hash tables[5] | 

## Download the MIT Weights from Hugging Face

The official repository is deepseek-ai/DeepSeek-V4.1-Flash on Hugging Face, published under the MIT license. Use huggingface-cli with the hf_transfer helper to pull roughly 511 GB of SafeTensors shards; the tokenizer and encoding utilities live in the same repo<sup>[4]</sup>.

Because the checkpoint is over half a terabyte, plan for both disk and network. hf_transfer parallelises shard downloads and is worth enabling on any connection faster than a gigabit<sup>[4]</sup>.

One practical note carried over from the V4-Flash era: DeepSeek does not ship a standard Jinja chat template. The repository includes an encoding/ directory with Python reference code and a Rust toolkit, and vLLM users sidestep the template entirely by passing --tokenizer-mode deepseek_v41<sup>[4]</sup><sup>[5]</sup>.

- Repo: deepseek-ai/DeepSeek-V4.1-Flash<sup>[4]</sup>
- License: MIT, commercial use permitted<sup>[4]</sup>
- Size: about 511 GB across SafeTensors shards<sup>[5]</sup>
- Extras: encoding/ Python reference plus a Rust toolkit<sup>[4]</sup>

Treat only huggingface.co/deepseek-ai as the official weights channel. DeepSeek has never published V4 weights on GitHub<sup>[4]</sup>.

```
pip install -U huggingface_hub hf_transfer
export HF_HUB_ENABLE_HF_TRANSFER=1
# Official MIT weights (~511 GB on disk)
huggingface-cli download deepseek-ai/DeepSeek-V4.1-Flash \
  --local-dir ./DeepSeek-V4.1-Flash
# Weights-only snapshot (skip docs, keep configs and tokenizer)
huggingface-cli download deepseek-ai/DeepSeek-V4.1-Flash \
  --include "*.safetensors" "*.json" "*.py" \
  --local-dir ./DeepSeek-V4.1-Flash
```
## Use the Official vLLM Docker Image

DeepSeek ships a purpose-built container, vllm/vllm-openai:deepseekv41-flash-0909, pinned to the September 9 build. It targets vLLM 0.30.0 or newer. There is no pip wheel for this recipe, because the model needs a vLLM build that knows the deepseek_v41 tokenizer, tool parser and reasoning parser<sup>[5]</sup>.

The image exists because V4.1 Flash is not a stock-architecture model. Its tokenizer mode, tool-call parser and reasoning parser are all registered under the deepseek_v41 name, so a generic vLLM install will not recognise the checkpoint<sup>[5]</sup>.

If you build vLLM from source instead of using the image, match the 0.30.0 line and confirm the deepseek_v41 components are present before downloading half a terabyte of weights<sup>[5]</sup>.

Pin the image tag. The deepseekv41-flash-0909 build is the combination DeepSeek validated on GB200 NVL4<sup>[5]</sup>.

```
docker run --gpus all --ipc=host --shm-size 64g \
  -v /models/DeepSeek-V4.1-Flash:/model \
  -p 8000:8000 \
  vllm/vllm-openai:deepseekv41-flash-0909 \
  --model /model
```
## The Complete vLLM Serve Command

The recipe's serve command sets four things that matter: the tokenizer mode, the tool-call parser, the reasoning parser and the multimodal encoder parallelism. On the GB200 reference rig, tensor parallelism is four<sup>[5]</sup>.

The --tokenizer-mode flag switches vLLM to DeepSeek's V4.1 tokenizer without needing a Jinja template. The two parser flags make the OpenAI-compatible server emit structured tool calls and separate reasoning content the way DeepSeek's hosted API does<sup>[5]</sup>.

The --mm-encoder-tp-mode data flag distributes the DeepSeek-ViT vision encoder, which matters because V4.1 Flash is natively multimodal: it accepts images alongside text, and the encoder must be sharded consistently with the language model<sup>[4]</sup><sup>[5]</sup>.

- --tokenizer-mode deepseek_v41 — required for the checkpoint
- --tool-call-parser deepseek_v41 — OpenAI-style function calls
- --reasoning-parser deepseek_v41 — separates thinking from output
- --mm-encoder-tp-mode data — shards the vision encoder
- --tensor-parallel-size 4 — reference value for GB200 NVL4<sup>[5]</sup>

Recommended sampling from the model card: temperature 1.0 and top_p 0.95 for agentic workloads, matching DeepSeek's own benchmark settings<sup>[4]</sup>.

```
vllm serve /model \
  --trust-remote-code \
  --tokenizer-mode deepseek_v41 \
  --tool-call-parser deepseek_v41 \
  --reasoning-parser deepseek_v41 \
  --mm-encoder-tp-mode data \
  --tensor-parallel-size 4 \
  --served-model-name deepseek-flash
```
## MXFP4 and MXFP8: What Ships in the Weights

DeepSeek distributes V4.1 Flash in a mixed low-precision format rather than BF16. The routed experts are quantized to MXFP4, most other weights use MXFP8 block quantization with UE8M0 scales, and the embedding and LM head stay in BF16<sup>[4]</sup>.

This mixed scheme is why the on-disk checkpoint is about 511 GB instead of multiple terabytes. Quantization-aware training means the published weights were trained with these formats in mind, so you are not bolting post-hoc quantization onto a BF16 model<sup>[4]</sup>.

The Engram conditional-memory tables are part of that footprint: 196.6B parameters split across two hash tables at layers 1 and 14, looked up by 4-gram hashing. The vLLM recipe accounts for 188.8 GiB of Engram plus 259.5 GiB of MXFP4 routed and DSpark expert weights<sup>[4]</sup><sup>[5]</sup>.

| Component | Precision | Notes | 
|---|---|---|
| Routed experts | MXFP4 | Largest weight block[4] | 
| DSpark draft experts | MXFP4 | Speculative draft head[5] | 
| Most other weights | MXFP8 | UE8M0 block scales[4] | 
| Embedding / LM head | BF16 | Kept high precision[4] | 
| Engram memory | Quantized tables | 196.6B params, 188.8 GiB[4][5] | 

## DSpark Speculative Decoding

V4.1 Flash bundles DSpark, a speculative decoding head that drafts tokens for the main model to verify. The draft module is a three-phase structure, each phase with 128 routed experts of which 3 activate, producing draft blocks of five tokens<sup>[5]</sup>.

DSpark is trained rather than bolted on: it lives in the same checkpoint and is quantized with the same MXFP4 treatment as the routed experts. In serving it works alongside normal decoding to raise throughput without changing the output distribution<sup>[5]</sup>.

For self-hosters, DSpark is the main lever on tokens per second. The recipe includes the draft weights in its memory budget, so if you disable speculative decoding you free that memory but also give up its speed benefit<sup>[5]</sup>.

## GGUF Alternatives and When to Call DeepSeek

If a 614 GB VRAM floor is out of reach, the community GGUF route is the fallback. A third-party quantization, AMAImedia/DeepSeek-V4.1-Flash-FP8-GGUF, exists on Hugging Face for teams running llama.cpp-style stacks<sup>[5]</sup>.

DeepSeek itself does not publish GGUF builds for V4.1 Flash, and it warns that re-quantizing native MXFP4 experts degrades quality. Community GGUF quants are therefore a convenience trade-off rather than an official path<sup>[4]</sup><sup>[5]</sup>.

For genuine production scale, DeepSeek's guidance is blunt: deployments involving around 2,000 GPUs plus a storage cluster should contact DeepSeek directly instead of assembling a recipe. Below that scale, the hosted API remains the most economical option for most teams<sup>[5]</sup>.
