---
id: collect-261121-veille-ia/modeles-open/reflection-beam-501b-is-open-on-hugging-face-teck-hustlers
title: "Reflection Beam 501B Is Open on Hugging Face | Teck Hustlers"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-16"]
keywords: ["beam", "hugging", "face", "open", "reflection", "reflections", "model", "nvfp4"]
source: https://teckhustlers.com/reflection-beam-501b-open-weight-huggingface/
source_anchor: ""
source_lines: [1, 50]
section: "Modeles_Open"
---
Reflection’s first open-weight model is no longer waitlist-only. On October 16, 2026, Hugging Face published Welcome Beam, confirming that **Beam 1.0 (501B-A23B)** weights are live under Apache 2.0. Reflection’s earlier Introducing Beam post described the same MoE design and coding focus; the Hugging Face drop is the moment self-hosters can actually download it. Here’s what the labs claim, what you need to run it, and how it sits next to other open releases we’ve covered — including Meta’s Muse Spark Open.

Beam is a text-in, text-out sparse mixture-of-experts model. The public numbers that both Reflection and Hugging Face repeat:

| Spec | Beam 1.0 (claimed) | 
|---|---|
| Total parameters | 501B (Hugging Face: ~501B MoE) | 
| Active per token | ~23B | 
| Context | 1M tokens | 
| Layers / experts | 52 layers; 256 experts, 8 routed per token (HF blog) | 
| Modalities | Text only | 
| License | Apache 2.0 | 
| Checkpoints | BF16, FP8, NVFP4 (+ DFlash2 drafter) | 

Reflection’s model card lists a June 30, 2026 knowledge cutoff and says Beam targets software engineering, terminal/DevOps work, MCP tool use and long-horizon agentic tasks. Those are Reflection’s stated goals, not independent lab reviews.

Western labs have spent 2026 racing Chinese open models on coding and agentic benches. Reflection says Beam is competitive with larger open models like GLM 5.2 on coding/agentic tasks while using less inference compute than multi-trillion-parameter peers. Treat benchmark tables as vendor-reported unless you reproduce them. Hugging Face republished Reflection’s BF16/FP8/NVFP4 scores for SWE-bench variants, Terminal Bench, AIME 2026, GPQA Diamond and tool-calling suites — useful as a starting map, not as a buying decision.

Two practical upsides of the open drop:

- **Self-host and customize.** Apache 2.0 plus SFT/RL recipes (TRL LoRA, Miles GRPO) means companies can fine-tune without waiting on a closed API.
- **Day-0 serving stack.** Transformers, vLLM and SGLang support landed with the release, including speculative decoding via DFlash2.

Hugging Face is blunt: the BF16 safetensors dump is about **1 TB**. NVFP4 shrinks most weights for Blackwell GPUs to roughly **280 GB**. Their guidance:

- **NVFP4 on Blackwell:** four B200s (or two B300s / 8× RTX PRO 6000) is a realistic serving plan; two B200s can hold weights with little room left for KV cache.
- **FP8 on Hopper:** use FP8 on 8× H200-class boxes if you don’t have Blackwell.
- **Try before you buy rack space:** Beam is on Hugging Face Inference Providers (Baseten, Fireworks, Together listed at launch).

If you’re comparing local open models that aim at smaller hardware footprints, Muse Spark Open’s Apple-focused pitch is a different product class than a 501B MoE — see our Muse Spark Open breakdown.

1. **Inference Providers:** call`reflection/Beam-1.0-501B-A23B:auto` through the Hugging Face router with an OpenAI-compatible client (HF’s blog shows the snippet).
2. **vLLM:**`vllm serve reflection/Beam-1.0-501B-A23B --tensor-parallel-size 16` is their BF16 example for two 8× H200 nodes; add DFlash for speculative decoding.
3. **SGLang:** NVFP4 on 4× B300 or FP8 on 8× H200 with Beam’s tool/reasoning parsers.
4. **Transformers:**`pipeline("text-generation", model="reflection/Beam-1.0-501B-A23B")` after installing a recent Transformers build (≥5.20.1 per HF).

Beam exposes reasoning effort levels (`low`, `medium`, `high`, `xhigh`, `max`) and a dedicated tool-call grammar. Hugging Face’s vibe evals showed higher reasoning effort fixing some long-context and math justifications that failed on low — still anecdotal, but useful when you tune prompts.

Beam lands in a week when open labs are racing on SWE-style tasks and long-running agents. If you’re still mapping the vocabulary, our guides to agentic AI and agentic vs generative AI explain why “tool-using coding model” is the pitch everyone is selling. Reflection’s angle is efficiency at inference time: big total parameter count, smaller active path per token, plus a Rust harness aimed at agent loops.

- **Vendor numbers.** Coding and agent scores come from Reflection (and HF republishing them). Independent third-party leaderboards will lag.
- **Text only.** No native vision; multimodal workflows need separate tooling.
- **Ops cost.** Even quantized, this is a multi-GPU service, not a laptop toy.
- **Safety.** Reflection says it ran deliberative alignment and will publish more in a technical report; review their model card and your own red-team plan before enterprise rollout.

In its October 5 preview post, Reflection said Beam was pretrained on 23.8 trillion tokens and that a high-compute RL stage used about 10.5K NVIDIA GB300 GPUs for four weeks, generating more than 100 million rollouts. Those figures are Reflection’s own disclosures. The company also said weights would ship under Apache 2.0 later in October — which is what Hugging Face delivered on the 16th. If you follow lab acquisition rumors separately, Teck Hustlers previously covered reported Nvidia–Reflection talks; today’s story is the model release itself, not M&A.

Hugging Face is also dangling $10,000 in Hub credits for strong community projects built on Beam (Spaces, quants, datasets, RL envs, benchmarks). That’s marketing, but it signals they expect an ecosystem burst around the checkpoint.

Beam’s open-weight release is one of the clearest Western coding-focused MoE drops of October 2026: 501B total, ~23B active, 1M context, Apache 2.0, and real day-0 serving support. Download it if you have the GPUs or a provider budget; otherwise watch independent evals before rewriting your stack around Reflection’s claims.
