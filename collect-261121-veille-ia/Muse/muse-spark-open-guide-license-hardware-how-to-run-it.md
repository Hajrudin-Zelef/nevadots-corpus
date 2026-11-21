---
id: collect-261121-veille-ia/muse/muse-spark-open-guide-license-hardware-how-to-run-it
title: "Muse Spark Open Guide: License, Hardware & How to Run It"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["muse", "model", "open", "spark", "license", "hardware", "meta", "parameters"]
source: https://codersera.com/blog/muse-spark-open-complete-guide-2026/
source_anchor: ""
source_lines: [1, 291]
section: "Muse"
---
**Quick answer.**Muse Spark Open is Meta's open-weight Mixture-of-Experts model, released October 15, 2026: 816B total and 42B active parameters, a 1M-token context, and text, image, video and audio input. The Muse Model License has no user cap, adds a $20M-revenue Model-as-a-Service clause, and requires a "Built with Muse Spark Open" credit. Hardware: ~2TB for BF16, 700GB+ for NVFP4, 180GB+ for GGUF quants, or two 512GB M5 Ultra Mac Studios via MLX. On OpenRouter it costs $1.25/$4.25 per million input/output tokens as of October 2026.

Meta released Muse Spark Open on October 15, 2026, and calls it "the most capable US open-weight model to date." It is Meta Superintelligence Labs' largest open-weight release so far, following the 30B Muse Glimmer. For context on that shift, see our piece on Meta's return to open weights. Unlike Muse Glimmer, a 30B dense model for consumer hardware, Spark Open is a frontier-scale model: 816 billion parameters, a 1M-token context window, and native multimodal input.

Engineering leaders want three things answered before they commit. Can we legally use this in a commercial product? What hardware do we need to run it? Is it good enough to replace a hosted API? The short answers: yes for most companies, with a mandatory attribution line and a revenue clause that only affects hosting businesses. The hardware runs from a pair of Mac Studios to a 2TB GPU cluster. And it leads on agentic tool-use benchmarks while trailing Kimi K3 and GLM 5.3 on coding.

This guide covers the architecture, the license in plain English, a hardware sizing table for every precision, how to run it with llama.cpp, vLLM or a hosted provider, the model-card benchmarks against Kimi K3 and GLM 5.3, and the API pricing with worked cost examples.

## Muse Spark Open at a glance

| Spec | Muse Spark Open | 
|---|---|
| Released | October 15, 2026 | 
| Developer | Meta Superintelligence Labs | 
| Hugging Face repo | `meta-models/Muse-Spark-Open-816B-A42B` (gated; requires accepting the license) | 
| Parameters | 816B total, 42B active per token (the Safetensors metadata lists 822B, and Unsloth cites about 42.7B active) | 
| Architecture | Mixture-of-Experts Transformer with a 1.8B-parameter Perception Encoder | 
| Experts | 256 experts, top-8 routing | 
| Context window | 1M tokens (1,048,576) | 
| Input / output | Text, image, video and audio in; text out | 
| Reasoning effort | `low` ,`medium` ,`high` ,`xhigh` (model-card default:`xhigh` ) | 
| Released precisions | BF16 and NVFP4 | 
| License | Muse Model License (not an OSI open-source license) | 
| Meta Model API | Not served; use self-hosting or a third-party provider | 
| OpenRouter ID | `meta/muse-spark-open` | 

## Architecture: what 816B-A42B actually means

The name encodes the important numbers: 816B total parameters, about 42B active (A42B). Each token passes through 8 of 256 experts, so per-token compute is closer to a 42B dense model than an 816B one. Memory is a different story. Every expert has to be resident (or quickly reachable), which is why the hardware requirements below track the 816B number.

From the Hugging Face model card, the language model has:

- **62 layers** with a hidden dimension of 8,192.
- **Grouped-query attention** with 128 query heads, 16 KV heads and a head dimension of 64. It uses gated attention (output gate and output norm) and QK-norm.
- **A 202,048-token vocabulary** with untied input and output embeddings, each about 1.66B parameters.
- **A Perception Encoder** for vision: 1.8B parameters, 50 layers, 14×14 patches. Sparse attention runs full attention on every fourth layer and the last layer.

The 16 KV heads matter for long-context serving. KV cache grows with context length, and Unsloth notes that longer contexts need memory beyond the weight footprint. Plan headroom if you intend to use the 1M window rather than a 32K-128K working context.

Meta positions the model for coding and agentic work. Its launch demos had it shipping iOS changes to the open-source Mastodon client Ice Cubes and driving DaVinci Resolve end to end, reviewing footage frame by frame to find and crop vertical clips. The open-weight release also got extra safety post-training on top of the hosted Muse Spark line. For the hosted family, see our Muse Spark guide.

## The Muse Model License, in plain English

The license matters more than the benchmarks for most buyers. Muse Spark Open is *open-weight*, not open source. Meta calls the Muse Model License a license with "limited commercial terms, which allow many developers to use the model for free." Here is what the license text on the model card says.

### What you can do

Use, copy, modify, fine-tune, create derivatives, distill, merge, publish, distribute, sublicense and sell, subject to the conditions below. Meta claims no ownership of outputs.

### The three conditions that matter

| Condition | What the license says | Who it affects | 
|---|---|---|
| No MAU cap | The license has no monthly-active-user threshold. Its only numeric trigger is revenue-based. | Large consumer apps, which could hit user caps under earlier Meta licenses | 
| Model-as-a-Service clause | If the licensee and its affiliates exceed twenty million US dollars in gross revenue in any consecutive 12-month period, they must notify Meta and sign a separate written agreement before offering the model or derivatives as a Model-as-a-Service business. Internal use, research, and use as a component of your own non-MaaS product are exempt. | Inference providers, AI platforms, and companies reselling model access to third parties | 
| Attribution | You must prominently display "Built with Muse Spark Open" in the user interface, documentation, website or settings menu of any product or service using the model or a derivative. | Everyone shipping a product on it, including fine-tunes | 

Redistributions must also include the license and the Usage Policy. The Usage Policy (Exhibit A) restricts risky areas including "Military, warfare, nuclear industries or applications, espionage" and "Operation of critical infrastructure, transportation technologies, or heavy machinery," along with the usual bans on malware, weapons, disinformation and impersonation. The model is not intended for users under 18.

### How to read the MaaS clause

The $20M figure is about *your company's* gross revenue, not revenue from the model. A SaaS company with $50M in revenue that embeds Muse Spark Open in its own product is using it "as a component of" its product, which the license exempts. The same company launching a general-purpose inference API built on the model would need Meta's separate agreement. If your product sits in between, for example a developer platform that exposes raw model calls, have counsel read Section 3 before you ship.

Gated access applies too. The Hugging Face download form asks for your full legal name, date of birth and full organization name.

## Hardware requirements: a sizing table

The numbers below come from the Hugging Face model card (BF16, NVFP4), Meta's launch post (Mac Studio) and Unsloth's documentation (GGUF quants). Unsloth's figures are total memory: RAM plus VRAM, or unified memory on Apple Silicon.

| Format | Memory needed | Typical target | Source | 
|---|---|---|---|
| BF16 (full precision) | ~2TB (weights alone ~1.63TB before runtime overhead) | Multi-node GPU cluster | Model card; Unsloth | 
| NVFP4 (Meta release) | 700GB+ VRAM | Single large multi-GPU node with NVFP4 support | Model card | 
| GGUF 8-bit | 870-950GB | Large-memory server, 2x 512GB Mac Studio (tight) | Unsloth | 
| GGUF 6-bit | 640GB | Large-memory server, 2x Mac Studio | Unsloth | 
| GGUF 4-bit | 512GB | 2x Mac Studio, CPU+GPU server | Unsloth | 
| GGUF 3-bit | 346-410GB | One 512GB Mac Studio, 384-512GB RAM server | Unsloth | 
| GGUF 2-bit | 250-316GB ( `UD-IQ2_XXS` fits in 256GB) | 256-512GB workstation or Mac | Unsloth | 
| GGUF 1-bit | 180-212GB | 256GB workstation or Mac (chat use only); the smallest ~180GB quant may squeeze into 192GB with minimal context | Unsloth | 
| MLX, whole model (quantized) | ~1TB unified memory across two machines | 2x M5 Ultra Mac Studio (512GB each) | Meta launch post | 

Two sanity checks on these numbers. First, BF16 is 2 bytes per parameter, and 816B × 2 bytes = 1,632GB, which matches Unsloth's ~1.63TB for the language-model weights. The gap between 1.63TB and the card's 2TB minimum is presumably headroom for KV cache and activations. Second, Unsloth notes that available memory should exceed the downloaded model size. llama.cpp can spill to disk or system RAM when it doesn't, but generation slows sharply.

### Quality versus quant size

Unsloth's KL-divergence testing found `UD-Q4_K_XL` very close to the original and `Q3_K_XL` reasonable. Quants below `UD-IQ2_S` degrade on general tasks. Unsloth recommends `UD-IQ2_S` as the minimum for agentic work and says the smallest quants are fine for chat-style use. For coding agents and tool calling, start at 3-bit or 4-bit if you have the memory.

### The two-Mac-Studio setup

Meta's launch post describes Muse Spark Open as "the first model in its class to run on your desktop using two Apple M5 Ultra Mac Studios" using Apple's MLX framework. Each M5 Ultra Mac Studio's integrated GPU can address up to 512GB of unified memory, with 1.2TB/s of memory bandwidth on the chip. Linked together, the two machines behave like a system with about a terabyte of unified memory. Meta's post doesn't state a precision, but a terabyte can't hold the ~1.63TB of BF16 weights, so this setup runs a quantized model: roughly 8-bit-class footprints at the tight end, or 4-bit comfortably.

By Unsloth's numbers, a *single* 512GB Mac Studio should hold the 2-bit or 3-bit GGUF quants with room for context. That is the cheaper entry point if you can accept the quality trade-off. For the networking, power and serving side of running Macs as inference boxes, see our guide to using a Mac as a local LLM server.

## How to run Muse Spark Open locally

The model card lists SGLang, vLLM, TensorRT-LLM, Hugging Face Transformers, Unsloth and llama.cpp as supported. The model card and Meta's post also cover Apple MLX on two M5 Ultra Mac Studios, and the post says custom Helion GPU kernels in SGLang gave 1.03-3.95× kernel-level speedups on NVIDIA and AMD GPUs and a 3% end-to-end speedup on AMD MI350. If you're choosing between runtimes, our comparison of Ollama, LM Studio, vLLM, llama.cpp and MLX covers the trade-offs.

### Option 1: llama.cpp with Unsloth GGUF

Support needed new model code. llama.cpp PR #30616, "model: add Muse Spark Open (text, vision and audio)", had to be merged before mainline builds could load the GGUFs. It was merged into `master` on October 15, 2026, so a fresh clone of `ggml-org/llama.cpp` includes it. If you're pinned to an older build, rebuild. Unsloth also points to prebuilt binaries in its own fork as a fallback.

Build (CUDA shown; Unsloth notes Metal is on by default on Macs, so set `-DGGML_CUDA=OFF` for Mac or CPU-only builds):

```
apt-get update
apt-get install pciutils build-essential cmake curl libcurl4-openssl-dev -y
git clone https://github.com/ggml-org/llama.cpp
cmake llama.cpp -B llama.cpp/build \
    -DBUILD_SHARED_LIBS=OFF -DGGML_CUDA=ON
cmake --build llama.cpp/build --config Release -j --clean-first \
    --target llama-cli llama-mtmd-cli llama-server llama-gguf-split
cp llama.cpp/build/bin/llama-* llama.cpp
```
Download the 2-bit quant Unsloth uses in its example (`UD-IQ2_XXS`, the smallest 2-bit quant, which fits in 256GB; use `UD-IQ2_S` or larger for agentic work):

```
pip install -U "huggingface_hub[cli]"
hf download unsloth/Muse-Spark-Open-816B-A42B-GGUF \
    --local-dir unsloth/Muse-Spark-Open-816B-A42B-GGUF \
    --include "*UD-IQ2_XXS*"
```
Run it, pointing at the first shard of the split GGUF:

```
./llama.cpp/llama-cli \
    --model unsloth/Muse-Spark-Open-816B-A42B-GGUF/UD-IQ2_XXS/Muse-Spark-Open-816B-A42B-UD-IQ2_XXS-00001-of-00006.gguf \
    --temp 1.0 \
    --top-p 1.0 \
    --top-k 0
```
Meta's model card recommends temperature 1.0, top_p 1.0 and top_k disabled, and Unsloth's parameter list matches. In llama.cpp, `--top-k 0` disables top-k. Unsloth's example command uses `--top-k 64`, which conflicts with its own recommendation, so we use the model-card setting here. To serve an OpenAI-compatible endpoint instead of an interactive CLI, use the `llama-server` binary from the same build with the same model path.

Unsloth also offers a one-liner through its own tooling: `unsloth run --model unsloth/Muse-Spark-Open-816B-A42B-GGUF:UD-IQ2_XXS`.

### Option 2: vLLM or SGLang on a GPU node

For production serving on GPUs, the model card's vLLM path is:

```
pip install vllm
vllm serve "meta-models/Muse-Spark-Open-816B-A42B"
```
On real hardware you'll add parallelism flags (tensor and pipeline parallel sizes) to match your node. The model card doesn't publish a reference configuration, so size against the 700GB+ NVFP4 or ~2TB BF16 figures above. SGLang works too: `pip install sglang`, then `python3 -m sglang.launch_server`. The card's Docker example uses `lmsysorg/sglang:latest` with `--gpus all --shm-size 32g --ipc=host`.

Reasoning effort is set per request. The model card says you can pass it as `reasoning_effort` or through `chat_template_kwargs`, and any value other than `low`, `medium`, `high` or `xhigh` returns an error. Against a local vLLM server's OpenAI-compatible endpoint:

```
from openai import OpenAI
client = OpenAI(base_url="http://localhost:8000/v1", api_key="EMPTY")
resp = client.chat.completions.create(
    model="meta-models/Muse-Spark-Open-816B-A42B",
    messages=[{"role": "user", "content": "Summarize the failing tests in this log: ..."}],
    temperature=1.0,
    top_p=1.0,
    extra_body={"chat_template_kwargs": {"reasoning_effort": "high"}},
)
print(resp.choices[0].message.content)
```
### What about Ollama?

As of October 2026, the Ollama library lists Muse Spark Open only under the `muse-spark-open:cloud` tag. No local tags or sizes are listed, so `ollama run` currently means Ollama's hosted inference, not your hardware. For local use, run llama.cpp, LM Studio or another runtime that loads the community GGUFs directly.

## Hosted providers and API pricing

Meta's own Model API doesn't serve Muse Spark Open. Its developer docs say "You don't call it over Model API." The same is true of Muse Glimmer. Meta lists these API partners:

- Baseten
- Fireworks AI
- Groq
- Modal
- Nebius
- OpenRouter
- Together AI

The weights are also available through Amazon SageMaker AI, Google Cloud Model Garden, Hugging Face and Microsoft Foundry Models.

### OpenRouter pricing (verified October 26, 2026)

OpenRouter's model API lists `meta/muse-spark-open` at **$1.25 per million input tokens**, **$4.25 per million output tokens** and **$0.15 per million cached input tokens**. Every upstream endpoint it routes to charges the same rate: Makora, Modal, Together, Nebius, Fireworks and Baseten. Makora, Nebius and Baseten report FP4 quantization, and the other three don't state one. Most endpoints offer the full 1,048,576-token context. Modal caps context at 1,000,000 tokens and max output at 262,144, and Makora caps output at 131,072. The two provider lists differ: Groq, a Meta launch partner, is not currently an OpenRouter endpoint, and Makora is not on Meta's partner list. Prices can change, so check the live page before budgeting.

One gotcha: OpenRouter's default reasoning effort for this model is `medium`, while Meta's model card defaults to `xhigh`, and all published benchmarks were run at `xhigh`. If you're comparing quality against the model card, set the effort explicitly:

```
from openai import OpenAI
client = OpenAI(base_url="https://openrouter.ai/api/v1", api_key="YOUR_OPENROUTER_KEY")
resp = client.chat.completions.create(
    model="meta/muse-spark-open",
    messages=[{"role": "user", "content": "Refactor this function to remove the N+1 query: ..."}],
    extra_body={"reasoning": {"effort": "xhigh"}},
)
print(resp.choices[0].message.content)
```
### Worked cost example

Take a coding-agent workload that processes 10M input tokens and 2M output tokens per day on OpenRouter:

- Input: 10M × $1.25/M = $12.50
- Output: 2M × $4.25/M = $8.50
- Daily total: $12.50 + $8.50 = **$21.00** , or $21.00 × 30 =**$630/month**

Agents resend the same repository context and system prompt over and over, so caching changes the math. If 70% of input tokens are cache hits:

- Cached input: 7M × $0.15/M = $1.05
- Uncached input: 3M × $1.25/M = $3.75
- Output: 2M × $4.25/M = $8.50
- Daily total: $1.05 + $3.75 + $8.50 = **$13.30** , or $13.30 × 30 =**$399/month**

Reasoning is mandatory on this model, and reasoning tokens generally count toward output, so `xhigh` runs will push the output line up. Measure your real output-to-input ratio before you compare this with self-hosting.

### When self-hosting makes sense

At a few hundred dollars a month, the API is cheaper than buying two top-spec Mac Studios or renting a 700GB+ GPU node. Self-hosting usually wins on something other than unit cost:

- **Data residency and privacy:** code or customer data that can't leave your network. Note that OpenRouter says this model isn't currently available through its EU in-region routing.
- **Fine-tuning and distillation:** the license allows it, and Meta supports fine-tuning through PyTorch and TorchTitan.
- **Very high, steady volume:** enough to keep dedicated hardware saturated around the clock.
- **Latency control:** no shared-tenant queues.

## Benchmarks: Muse Spark Open vs Kimi K3 and GLM 5.3

All numbers below are **vendor-reported** from Meta's Hugging Face model card. Muse Spark Open ran at `xhigh` effort, temperature 1.0 and top_p 1.0. Meta lists the comparison models as GLM 5.3 at "max" and Kimi K3 at "max". Treat these as Meta's measurements until independent evaluations land. Meta says a full evaluation report is coming. For the competitors' own claims, see our Kimi K3 guide and GLM 5.3 launch guide.

| Benchmark (as named in the model card) | Muse Spark Open | GLM 5.3 | Kimi K3 | 
|---|---|---|---|
| Artificial Analysis Intelligence Index | 39 | 45 | 44 | 
| MCP-Atlas | **89.2** | 84.2 | 82.3 | 
| JobBench | **61.8** | 61.4 | 54.3 | 
| OSWorld Verified | 80.4 | not reported | **84.8** | 
| Toolathlon Verified | 72.8 | 73.0 | **76.5** | 
| GDPVal-AA v2 | 1401 | **1653** | 1537 | 
| HLE-Diamond (no tools) | **26.1** | 16.4 | 22.2 | 
| HLE (with tools) | 61.0 | **62.5** | 56.0 | 
| GPQA Diamond | 89.0 | 91.7 | **93.5** | 
| AA Omniscience | **25** | 14 | 20 | 
| SciCode | 58.5 | 59.0 | **59.5** | 
| DeepSWE 1.1 (preferred harness) | 60 | 67 | **68** | 
| SWE-Atlas-QnA (preferred harness) | 58 | 59 | **66** | 
| Terminal-Bench 2.1 (Terminus 2 agent) | 76.4 | 83.9 | **85.0** | 

### How to read this

- **Tool use and MCP:** MCP-Atlas is Muse Spark Open's clearest lead, 89.2 against 84.2 for GLM 5.3 and 82.3 for Kimi K3. If your workload is an agent calling many MCP tools, this is the number to test against.
- **Computer use:** 80.4 on OSWorld Verified is well ahead of Thinking Machines' Inkling (40.1), another US open-weight MoE in the same table, but it trails Kimi K3's 84.8. GLM 5.3 has no reported score.
- **Coding:** this is the weak spot. Muse Spark Open trails both rivals on DeepSWE 1.1 (60 vs 67 and 68), SWE-Atlas-QnA and Terminal-Bench 2.1 (76.4 vs 83.9 and 85.0). For a pure coding agent, GLM 5.3 or Kimi K3 still look stronger on Meta's own numbers.
- **Overall:** on the Artificial Analysis Intelligence Index, as reported in the model card, it scores 39 against 45 (GLM 5.3) and 44 (Kimi K3).
- **Safety:** on StrongREJECT v2 (adaptive), where lower is better, the model card reports 0.5 for Muse Spark Open against 62.9 for GLM 5.3 and 66.6 for Kimi K3. That matters for regulated deployments. It also means you should test for over-refusal on legitimate security work.

## Should your team adopt it?

| Situation | Recommendation | 
|---|---|
| Agent platform with heavy MCP or tool use | Strong candidate. Benchmark it on MCP-Atlas-style tasks against your current model. | 
| Pure coding agent (SWE-style tasks) | Test it, but Kimi K3 and GLM 5.3 lead on Meta's own coding numbers. | 
| Need a US-origin open-weight model for compliance reasons | A strong reason to pick it: Meta calls it the most capable US open-weight model to date. Plan for the attribution line and the Usage Policy restrictions. | 
| Startup or SaaS embedding it in your own product | License-friendly. The MaaS clause doesn't apply to component use. | 
| Inference provider or AI platform with >$20M revenue | Contact Meta before launch. A separate agreement is required. | 
| Small team, modest volume | Use a hosted provider at $1.25/$4.25 and self-host later if needed. | 

## FAQ

### Is Muse Spark Open open source?

No. The weights are downloadable, and the license allows commercial use, fine-tuning, distillation and redistribution. But the Muse Model License adds conditions an OSI license wouldn't: mandatory "Built with Muse Spark Open" attribution, a Model-as-a-Service revenue clause, and a Usage Policy with restricted uses.

### Is there a monthly active user limit?

No. The license has no MAU threshold. Its only numeric trigger applies to Model-as-a-Service businesses whose company-wide gross revenue exceeds $20M in any consecutive 12-month period.

### Can I run Muse Spark Open on one Mac?

A single 512GB M5 Ultra Mac Studio should fit Unsloth's 2-bit (250-316GB) or 3-bit (346-410GB) GGUF quants. The configuration Meta highlights for running the whole model on desktops uses two M5 Ultra Mac Studios (512GB each) with MLX, at quantized precision, since about 1TB of combined memory can't hold the ~1.63TB of BF16 weights.

### Does llama.cpp support it?

Yes. Support landed in llama.cpp PR #30616, merged on October 15, 2026. Builds from before that merge can't load the model, so rebuild from current `master`.

### Can I run it with Ollama?

As of October 2026, Ollama's library lists only a `:cloud` tag for Muse Spark Open, which runs on Ollama's servers. For local inference, use llama.cpp or another GGUF-compatible runtime.

### Is it available on Meta's Model API?

No. Meta's developer docs say Muse Spark Open isn't served on the Model API. Use Baseten, Fireworks, Groq, Modal, Nebius, OpenRouter or Together AI, or host it yourself.

### What does it cost via API?

On OpenRouter, as of October 26, 2026: $1.25 per million input tokens, $4.25 per million output tokens, and $0.15 per million cached input tokens.

## Sources

If you're planning to self-host Muse Spark Open, from sizing GPU nodes to tuning vLLM or llama.cpp serving, Codersera can help you extend your engineering team with vetted remote AI developers experienced in self-hosted LLM inference: GPU sizing, vLLM and llama.cpp serving, and fine-tuning. Hire AI developers with the right technical fit, faster.
