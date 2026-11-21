---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-local-hardware-vram-files-and-licence
title: "Kimi K3.1 Pro Local: Hardware, VRAM, Files and Licence"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-13"]
keywords: ["kimi", "model", "none", "hardware", "licence", "internal", "moonshot", "repository"]
source: https://vdf.ai/blog/kimi-k3-1-pro-local/
source_anchor: ""
source_lines: [1, 157]
section: "Kimi"
---
Kimi K3.1 Pro can run locally, but only on data-centre hardware. The open checkpoint on Hugging Face is 1.56 TB of MXFP4 weights, and vLLM's baseline recipe needs eight large GPUs: one B300 node, two GB300 trays or one AMD MI355X node. The Kimi K3.1-Pro License allows self-hosting and exempts internal use from its commercial conditions. No consumer GPU or single Mac can hold it, and Ollama's kimi-k3.1-pro tag runs in Ollama's cloud.

Moonshot AI launched Kimi K3.1 Pro in its apps and API in late October 2026: launch coverage dates it to 23 October, and the tech blogâs page metadata carries a 24 October date. The blog promised the weights âby Nov 4â. The Hugging Face repository was created privately on 2 November, its first public discussions and community conversions appeared on 4 November, and its latest commit, on 5 November, updated `encoding_k3.py`. A faster Highspeed variant exists on the API only, and there is no separate non-Pro âKimi K3.1â model.

For the original model, see our Kimi K3 on-premises guide, which covers the July release, its licence and multi-node serving in depth. This article sticks to what is new or different in K3.1 Pro, as checked against Moonshotâs model card, licence and file listing, the vLLM and SGLang recipes and the Kimi API price list on 13 November 2026.

## What is Kimi K3.1 Pro? Release, architecture and changes since K3

Moonshot calls K3.1 Pro âthe first model in the K3.1 seriesâ and describes it as K3 post-trained with âsubstantially larger post-training computeâ, using joint reinforcement learning across hundreds of domains. The underlying network is the same size, so anyone who sized hardware for K3 can reuse that plan.

| Property | Kimi K3.1 Pro | Change from K3 | 
|---|---|---|
| Parameters | 2.8T total, 104B activated | None | 
| Experts | 896 routed, 16 per token, plus 2 shared | None | 
| Layers | 93: 69 Kimi Delta Attention + 24 Gated DSA | Gated MLA replaced by Gated DeepSeek Sparse Attention | 
| Sparse attention | Indexer with 32 heads of 128 dimensions picks the top 2,048 tokens | New | 
| Context window | 1,048,576 tokens | None | 
| Input | Text and images through MoonViT-V2 (401M) | None | 
| Weights | MXFP4 routed experts, BF16 elsewhere, from quantisation-aware training | None | 
| Reasoning | Always on; `reasoning_effort` low, high or max | None | 

The mixture-of-experts design is why the active count looks modest while the memory bill does not: each token touches about 104 billion parameters, but all 2.8 trillion must stay resident. Video goes through K3âs shared multimodal processor, which SGLang supports by sampling clips into timestamped four-frame chunks; Moonshotâs card itself lists text and image.

Moonshotâs headline figures are vendor-reported and use its own harness choices. K3.1 Pro scores 65.7% on Terminal-Bench 4.0 and 61.2% on the internal Kimi Code Bench 3.0 Long-horizon, against 55.95% for K3. On its internal hallucination stress test, the claim-level error rate falls from 23.4% to 8.3%.

### Cyber-related training data removed before the open release

The tech blog describes a deliberate cut. Moonshot traced rising vulnerability-research ability to cyber content in the mid-training corpus and to cyber tasks in the RL mix, then removed both for the open-weight release while keeping general coding tasks. On its internal xdev-bench-hard, the checkpoint without those measures solved 90.5% of tasks at pass@3 and the released model 81.0%. Moonshot says the released model still trails frontier proprietary models on offensive cyber work but improves on K3.

### Where the K3.1 technical details are published

There is no K3.1 GitHub repository or technical report; Moonshotâs GitHub organisation still lists Kimi-K3 as its latest model repository. The architectural detail comes from the model card, `config.json` and the engine recipes, which is enough for deployment work.

## Kimi K3.1 Pro on Hugging Face: files and sizes by precision

The official repository holds 96 safetensors shards totalling 1,561.4 GB, about 1,454 GiB, plus a 59.8 MB weight index. Hugging Face counts 2.78 trillion stored parameters: 2.72 trillion in packed 4-bit expert tensors and 57.4 billion in BF16. Moonshot publishes no BF16 or FP8 checkpoint. The MXFP4 file is the trained precision, not a conversion, so a âhigher-precisionâ community quantisation cannot recover anything that is not in it.

| Build | Size | Publisher | Purpose | 
|---|---|---|---|
| Official MXFP4 | 1,561.4 GB | Moonshot AI | Reference checkpoint for vLLM, SGLang and TokenSpeed | 
| Mirror | 1,561.4 GB | Unsloth | Unchanged copy | 
| NVFP4 | 1,605.2 GB | NVIDIA | Blackwell only; routed experts converted to NVFP4 with Model Optimizer 0.48.0 | 
| Pruned 75% | 475.5 GB | vLLM project | 224 of 896 experts kept; for smoke tests on four GPUs | 
| GGUF UD-Q4_K_XL / UD-Q8_K_XL | 1,509.0 / 1,561.5 GB | Daniel Han (preliminary) | llama.cpp | 
| GGUF Q2_K / MXFP4_MOE | 1,009.3 / 1,508.9 GB | DevQuasar | Marked âvery experimentalâ | 
| GGUF KLX-IQ1_S / IQ1_M / IQ2_XXS | 592.7 / 647.9 / 709.9 GB | KarLexAI | Fitting one 8Ã H100 node | 
| EXL3 2-bit | 797.0 GB | local-inference-lab | Eight RTX PRO 6000 Blackwell cards | 
| DSpark / DFlash2 draft | 5.4 / 5.6 GB | Inferact / RadixArk | Speculative decoding drafts | 

Two details matter before downloading. NVIDIAâs NVFP4 card puts the model under the NVIDIA Open Model Agreement in addition to Moonshotâs licence, and its own evaluation shows near-identical scores to MXFP4, such as 93.84 against 93.97 on GPQA Diamond. vLLMâs pruned checkpoint scores 43.67% on GSM8K and its card warns of âa large quality regressionâ, so it suits pipeline tests, not users. Our quantisation guide for on-premises GPUs explains why sub-2-bit builds need your own evaluation before anyone relies on them.

The repository ships Python for the configuration, tokenizer, image processor and chat encoding, but, unlike K3, no Transformers modelling file: the weights load through engine code. Commands still pass `--trust-remote-code` for those helpers, so review those files and pin the commit you reviewed; `d9e99b0` was current on 13 November.

## Kimi K3.1 Pro licence: self-hosting, revenue thresholds and attribution

The Kimi K3.1-Pro License is word for word the Kimi K3 licence with the model name changed in the title and the display clause; we compared the two files directly. It grants MIT-style rights to use, copy, modify, distribute, sublicense, fine-tune and deploy the software, which it defines to include the weights, and adds:

- **A Model-as-a-Service clause.** Selling third parties inference or fine-tuning with meaningful control over inputs, parameters or training data, through an API for example, is Model-as-a-Service. If you or any affiliate run such a business and combined revenue exceeds US$20 million over any 12 consecutive months, you need a separate agreement with Moonshot AI before commercial use. Features embedded in an end-user product, and relaying requests to models hosted elsewhere, are excluded.
- **An attribution clause.** Commercial products or services with more than 100 million monthly active users, or more than US$20 million in monthly revenue, must display âKimi K3.1-Proâ prominently in the interface.
- **Exemptions.** Neither clause applies to internal use, meaning use that does not make the model, its outputs or its capabilities available to third parties, or to access through Moonshotâs own products and certified inference partners.

An enterprise serving its own staff falls under the internal-use exemption. The group-level revenue test is what legal teams should check, because one cloud or API subsidiary can bring the whole group into scope. Derivatives keep these terms: vLLMâs pruned build and the KarLexAI GGUFs restate the licence, while DevQuasarâs repository declares none, which is a provenance gap rather than permission. Our open-weight licence checklist lists the wider questions; none of this is legal advice.

## Kimi K3.1 Pro hardware requirements: VRAM per node

The arithmetic starts with 1,561 GB of weights, then adds cache, activations, the vision tower and runtime buffers. Use our LLM VRAM calculator for the general method; the figures below come from vendor capacities and the engine recipes.

| Hardware | GPU memory | Holds the official weights? | Status on 13 November 2026 | 
|---|---|---|---|
| 8Ã B300 (DGX B300) | 2.1 TB | Yes, about 0.54 TB spare | vLLM baseline with tuned presets; SGLang verified on the decode role | 
| 2 GB300 trays, 8 GPUs | About 2.2 TB | Yes | vLLMâs default configuration | 
| 8Ã MI355X | 2,304 GB | Yes | vLLM day-0 ROCm image; SGLang on ROCm 10, verified | 
| 16Ã H200, two nodes | 2,256 GB | Yes, split across nodes | SGLang placeholder layout, not verified | 
| 16Ã B200, two nodes | 2,880 GB | Yes, split across nodes | SGLang placeholder layout, not verified | 
| 32Ã H100, four nodes | 2,560 GB | Yes, split across nodes | SGLang placeholder layout, not verified | 
| 8Ã H200 or 8Ã B200, one node | 1,128 or 1,440 GB | No | Needs a second node or a community quantisation | 
| 8Ã RTX PRO 6000 | 768 GB | No | Community EXL3 2-bit only, 262,144-token limit | 
| 8Ã H100, one node | 640 GB | No | Community 1-bit GGUF only | 
| 16-chip TPU v7x slice | Not applicable | Yes | vLLM and SGLang TPU paths, without image input | 

The cache is smaller per token than in many dense models, but it is not free. SGLangâs cookbook gives about 18.9 KB per token for the sparse-attention latent and indexer keys, plus about 52 MB of FP32 Kimi Delta Attention state per request on each GPU at TP8. Under plain tensor parallelism that cache is copied to every GPU. vLLM measured about 2.6 million cached tokens across eight B300s at TP8 with an FP8 cache, and about 18.5 million with decode context parallelism (DCP8), which shards the cache instead. An optional NVFP4 cache stores 352 bytes per token per layer instead of 576 and holds about 1.5 times as many tokens; vLLM has not yet measured its long-context accuracy.

Host memory matters too. vLLMâs KV-offload presets pin between 128 GiB and about 262 GiB of system RAM per GPU, so a node with eight GPUs may need more than a terabyte of DRAM for agent-heavy work. SGLang notes a 1,511 GiB pinned-memory limit on MI355X hosts. Storage throughput sets cold-start time: vLLMâs commands raise the engine-ready timeout to an hour, or two hours with the low-latency decode presets.

On smaller hardware, the honest answer is a smaller model. Our ranking of local LLMs by VRAM lists what fits from 8 GB cards to 8-GPU servers, our walkthrough of DeepSeek V4.1 Flash covers a single-node alternative, and our RTX PRO 6000, H100 and H200 comparison helps when buying for several models.

## How to serve Kimi K3.1 Pro with vLLM and SGLang

Moonshotâs card recommends three engines: vLLM, SGLang and TokenSpeed. All three expose an OpenAI-compatible API. vLLM and SGLang both still served K3.1 Pro from day-0 or nightly builds rather than tagged releases on 13 November.

### Kimi K3.1 Pro on vLLM

The vLLM recipe requires vLLM 0.34.0 or later, ânot released yetâ, so use a nightly wheel or the `vllm/vllm-openai:kimi-k31` image; PyPIâs latest release was 0.33.0, published on 2 November. AMD uses `vllm/vllm-openai-rocm:kimi-k31`, and Vera Rubin previews use a CUDA 13.4 image. The default command runs one eight-way tensor-parallel group over two GB300 trays. This is the head node; the worker adds `--node-rank 1` and `--headless`:

```
export VLLM_ENGINE_READY_TIMEOUT_S=3600
vllm serve moonshotai/Kimi-K3.1-Pro \
  --trust-remote-code \
  --tensor-parallel-size 8 \
  --nnodes 2 --node-rank 0 --master-addr $HEAD_IP \
  --load-format fastsafetensors \
  --enable-auto-tool-choice \
  --tool-call-parser kimi_k3 \
  --reasoning-parser kimi_k3
```
Beyond the baseline, the recipe documents decode context parallelism for long sessions, a low-latency decode path, host KV offload, prefill/decode disaggregation over NIXL and a K3.1-specific DSpark speculative draft. It also states a precision policy: BF16 all-reduce and MoE combine, and an FP32 recurrent state for the Kimi Delta Attention layers. Its published timings come from a pre-release checkpoint on eight B300s. A single 340,000-token prompt prefilled in about 10 seconds with tensor plus expert parallelism, against about 15 seconds at plain TP8, and the low-latency path cut each decode step by 1.6 to 1.7 times at batch size 1.

### Kimi K3.1 Pro on SGLang

The SGLang cookbook serves K3.1 Pro from a `kimi-k3.1-day-0` branch or the `lmsysorg/sglang:dev-kimi-k31-pro` image, with a separate MI350X and MI355X image for ROCm 10. It forces an FP8 KV cache with a page size of 64 and offers Low-Latency (TP8), High-Concurrency (DCP8), High-Throughput and prefill/decode shapes. Only the B300 decode cells and the AMD cells were marked Verified; everything else was âin verificationâ. On a 16-chip TPU7x slice, SGLang publishes 133 tokens per second for a single request at 8K input and 112 at 128K.

### llama.cpp, community runtimes and Ollama

llama.cpp merged K3.1 text support on 6 November 2026 (pull request #31779) and vision support on 10 November (#31980), which is what the GGUF builds rely on. KarLexAI says its 592.7 GB IQ1_S file runs entirely on one 8Ã H100 node with 131K tokens of context at 19 tokens per second; those are the publisherâs own figures. Ollama is the trap for anyone searching âkimi k3.1 localâ: its library lists only `kimi-k3.1-pro:cloud`, a hosted model billed per token. Our comparison of vLLM, Ollama, llama.cpp and SGLang sets out when each engine makes sense, and our sibling article on NVIDIA NIM, vLLM and Ollama adds the packaged option.

### Preserved thinking in multi-turn calls

K3.1 Pro always reasons. For multi-turn chats and tool calls, the model card says the complete assistant message, including `reasoning_content` and `tool_calls`, must go back into `messages` unchanged. Any proxy, SDK or agent framework that drops reasoning between turns will degrade results, so test that path before go-live.

## Kimi K3.1 Pro API vs self-hosting: prices and trade-offs

Moonshotâs price list on 13 November 2026, per million tokens, for models with a 1,048,576-token window:

| Model | Input | Output | Cached input | 5-minute cache write | 1-hour cache write | 
|---|---|---|---|---|---|
| kimi-k3.1-pro | $2.00 | $10.00 | $0.20 | $2.00 | $4.00 | 
| kimi-k3.1-pro-highspeed | $4.00 | $20.00 | $0.20 | $4.00 | $8.00 | 
| kimi-k3 | $3.00 | $15.00 | $0.30 | $3.00 | $6.00 | 

There is no context-length tiering, batch jobs cost 60% of standard prices, and the model unlocks after a minimum $1 top-up. Moonshot says Highspeed delivers two to three times K3.1 Proâs speed, and it offers zero data retention to API customers. Kimi Enterprise administrators must switch K3.1 Pro on, since it is off by default. Ollamaâs cloud tag charges the same $2 and $10.

At those rates, a team generating 500 million output tokens a month pays $5,000 in output fees before input and caching, which is small next to the cost of an eight-GPU Blackwell node. Self-hosting K3.1 Pro rarely wins on token price alone; it wins when prompts, code and documents must not leave your network, when an air-gapped site has no API to call, or when you need a fixed model version you can audit.

- **Self-hosted:** no data leaves your hardware, the version stays pinned and you control logs and retention. It needs a node costing far more than most API bills, nightly engine builds and staff who can run multi-GPU inference.
- **API:** pay per token with Highspeed and batch options and nothing to operate. Every prompt reaches Moonshotâs infrastructure, even with zero retention.

For most organisations a routed mix is the sensible middle: a single-node model for everyday traffic, and K3.1 Pro only for the long-horizon coding and research tasks where your own evaluation shows a gain. Our shortlist of local coding models and our GPU sizing method help price that mix.

## How VDF AI fits

VDF AI does not run, host, bundle or fine-tune Kimi K3.1 Pro. If you serve it yourself, the model is one endpoint among several, and the work around it is what VDF AI handles: governed AI agents, multi-agent workflows in VDF AI Networks, private RAG over company documents, role-based access control on every plan and audit logs, all on your own infrastructure.

VDF AI Router accepts OpenAI-compatible endpoints as model sources, which is how vLLM, SGLang and TokenSpeed expose K3.1 Pro. It registers those deployments, and Ollama models, next to the cloud models your policy approves and prefers the local ones when they are available; its air-gap mode keeps routing on-premises. Because K3.1 Pro depends on preserved reasoning across turns, a pilot should confirm that multi-step tool calls keep `reasoning_content` intact end to end.

Production self-hosting of VDF AI runs on Linux, with macOS for evaluation only; the infrastructure requirements list the details.

## Sources

Kimi K3.1 Pro facts below were verified against these pages on 13 November 2026.

- Moonshot AI: Kimi-K3.1-Pro model card, Kimi K3.1-Pro License, file listing, commit history, config.json, tech blog, API quickstart and model pricing
- For comparison: Kimi-K3 licence and Kimi-K3 config
- Serving: vLLM recipe for Kimi-K3.1-Pro, SGLang cookbook for Kimi-K3.1-Pro, TokenSpeed model recipes, vLLM on PyPI, llama.cpp pull request #31779 and #31980, and Ollamaâs kimi-k3.1-pro page
- Community and partner builds: NVIDIA NVFP4, vLLM pruned75, Unsloth mirror, preliminary GGUF, DevQuasar GGUF, KarLexAI GGUF, EXL3 2-bit, DSpark draft and DFlash2 draft
- Hardware: NVIDIA DGX B300, DGX B200 and H200

**Weighing a 2.8-trillion-parameter model against what your GPUs can hold?** See how VDF AI Router sends each request to the approved on-premises endpoint that fits it, or talk to our team.
