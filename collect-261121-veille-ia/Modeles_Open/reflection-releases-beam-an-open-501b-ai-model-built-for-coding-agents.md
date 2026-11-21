---
id: collect-261121-veille-ia/modeles-open/reflection-releases-beam-an-open-501b-ai-model-built-for-coding-agents
title: "Reflection releases Beam, an open 501B AI model built for coding agents"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["beam", "model", "reflection", "context", "layers", "parameters", "experts", "built"]
source: https://unicornapi.net/blog/reflection-releases-beam-open-501b-ai-model-coding-agents
source_anchor: ""
source_lines: [1, 123]
section: "Modeles_Open"
---
# Reflection releases Beam, an open 501B AI model built for coding agents

Unicorn News Desk ·

## Key facts

- Maker
- Reflection (new US lab)
- Total parameters
- 501B, about 23B active per token
- Context window
- 1M tokens
- Input/output
- Text in, text out
- Licence
- Apache 2.0

Reflection, a new US AI lab, has released Beam, its first model, as open weights on Hugging Face under the Apache 2.0 licence. Beam is a 501-billion-parameter mixture-of-experts model that runs about 23 billion parameters per token, takes text only, offers a 1M-token context window and targets coding and agentic work.

The release post, written by Hugging Face with the Reflection team and published on October 16, presents Beam as a model for teams that want a frontier-scale model on their own hardware, infrastructure or cloud, and that want to customise it. Reflection’s Hugging Face collection calls Beam 1.0 “Reflection’s first open-weight model, built for coding, reasoning, and agentic tasks”. The weights follow an announcement on October 5, when Reflection said Beam was still in final red-teaming and evaluation with a select early-access group.

## How Beam is built: a large model that runs like a smaller one

Beam is a sparse mixture-of-experts (MoE) model. In an MoE, each layer holds many smaller sub-networks, called experts, and a router sends each token to only a few of them. Hugging Face describes the result as a model that “runs at the speed of a much smaller model while keeping the capacity of a large one”.

The first 3 of Beam’s 52 layers use a standard dense block. The other 49 each hold 256 experts of about 39M parameters, and together the experts account for 98.4% of the model’s parameters. The router follows DeepSeek-V3’s group-limited scheme: it scores all 256 experts, splits them into 8 groups of 32, keeps the 4 best groups and picks the top 8 experts from those. Beam uses the same group sizes as DeepSeek-V3 but has no shared expert.

The trade-off matters for deployment. All 256 experts must stay in memory, so Beam “needs the memory of a 501B model but runs at the speed of a 23B one,” according to the post. The BF16 checkpoint is about 1 TB.

### Attention and long context

Most of Beam’s layers use sliding-window attention, which means each token attends only to the previous 512 tokens. Every sixth layer, and the last one, attends to the full context. That gives 43 sliding-window layers and 9 global ones. Mixing many cheap local layers with a few full-context layers is a common way to keep long-context inference affordable.

The two layer types handle position differently. Sliding-window layers use RoPE, a standard positional encoding. Global layers use no positional encoding. Instead, they apply what the implementation calls “scalable-softmax query scaling,” a factor that grows slowly with context length so attention does not flatten out as the context gets longer. Attention uses grouped-query attention with 48 query heads and 8 key-value heads.

Hugging Face singles out the residual stream as Beam’s most distinctive feature. A standard transformer adds each sublayer’s output to the running stream with weight 1, so the stream grows with depth. Beam re-weights each addition so the stream stays at a constant scale from the first layer to the last. After all 104 additions, every sublayer and the input embedding carry equal weight in the final state.

## Beam’s specs at a glance

| Spec | Beam 1.0 | 
|---|---|
| Total parameters | 501B | 
| Active parameters per token | About 23B | 
| Layers | 52 (43 sliding-window, 9 global attention) | 
| Experts | 256 per MoE layer, 8 routed per token | 
| Modalities | Text in, text out | 
| Context window | 1M tokens | 
| Checkpoint formats | BF16, FP8, NVFP4 | 
| Reasoning settings | low, medium (default), high, xhigh, max | 
| Speculative decoding drafter | DFlash2, about 1.9B parameters | 
| Licence | Apache 2.0 | 

Beam also supports reasoning that can stay in context across conversation turns, through a setting called *preserve_thinking*, and tool calling with a dedicated grammar. Speculative decoding, which Beam supports through the DFlash2 drafter, uses a small, fast model to propose tokens that the large model then checks, which can speed up generation.

## Where Beam is available and under what licence

The weights are on Hugging Face under Apache 2.0, a permissive licence that allows commercial use. Reflection’s collection includes the main BF16 model, an FP8 version, an NVFP4 version and the DFlash2 drafter. None of the listed models are gated.

The formats target different hardware. The NVFP4 checkpoint stores most parameters in 4-bit precision for Nvidia Blackwell GPUs, with some kept in BF16 for accuracy. The FP8 checkpoints are described as ideal for Hopper and Ampere chips.

For self-hosting, Beam has a native implementation in the transformers library and day-zero support in the vLLM and SGLang inference engines. Hugging Face and Reflection have published recipes for inference, supervised fine-tuning and reinforcement learning (RL), including full-parameter RL with the Miles framework. Beam also ships with its own open-source, Rust-based harness that, according to the post, “delivers the best performance from the model”.

For teams that do not want to host the model, Reflection’s Hugging Face listings show hosted inference through Baseten for the main model and through Together for the NVFP4 version. Both listings show tool calling support. Reflection’s Hugging Face page also points to its own developer docs and a Reflection platform. The brief for this story mentioned day-one support on Google Cloud, but none of the sources reviewed confirm it, so it is not reported here.

Reflection is also offering $10,000 in Hugging Face Hub credits to the best community projects built with Beam, such as demos, quantised versions, synthetic datasets, RL environments or benchmarks.

## Reflection’s claims versus independent evidence

The sources contain no independent benchmark results for Beam yet. What exists falls into two groups.

**Maker claims.** According to ELYMENT AI’s summary of Reflection’s October 5 announcement, Reflection says Beam was pretrained on 23.8 trillion tokens and trained with more than 100 million reinforcement-learning rollouts. Reflection publishes results across coding, terminal, reasoning, tool-calling and search benchmarks. ELYMENT AI also notes that Reflection’s own compute comparisons are estimates that leave out prompt prefill, context-dependent attention and serving overhead. In other words, the efficiency figures are not the same as measured cost in a production stack.

**Observable signals.** On Reflection’s Hugging Face page, the main Beam checkpoint showed 7,733 downloads and 286 likes at the time of the snapshot. The same listing showed throughput of about 320 tokens per second on Baseten and about 124 tokens per second for the NVFP4 version on Together. These figures come from a live listing, change over time and say nothing about output quality.

### A discrepancy on context length

The sources differ on long context. Hugging Face’s release post lists a 1M-token context window. ELYMENT AI’s summary of the earlier announcement says Reflection reported a maximum context length of 256,000 tokens during training. The two figures may describe different things, a trained maximum and a supported window, but the sources do not reconcile them. Teams that plan to rely on very long inputs should test retrieval and reasoning quality near the top of the range themselves.

ELYMENT AI also notes that Reuters reported Beam’s debut as an open-weight challenger for coding and agentic tasks.

## What Beam means for teams building coding agents

For developers, Beam is notable less for any single benchmark than for the combination it offers. It is a large, permissively licensed model aimed at long-horizon coding and agent work, and it comes with fine-tuning and RL recipes from day one. A few practical points stand out:

- **Memory sets the hardware floor, not speed.** Per-token compute is roughly that of a 23B model, but the whole 501B model must fit in memory. The roughly 1 TB BF16 checkpoint means multi-GPU serving. The FP8 and NVFP4 versions are the realistic starting points for most teams.
- **Agent features are built in.** Tool calling with a dedicated grammar, reasoning that persists across turns and five reasoning-effort levels are all aimed at multi-step agent loops.
- **The custom harness matters.** Reflection’s Rust harness is positioned as the way to get the best performance. Teams using their own agent frameworks should check whether results differ outside it.
- **Customisation is the pitch.** The supervised fine-tuning and full-parameter RL recipes target teams that want to adapt the model to their own codebases or tools.

ELYMENT AI’s guidance, written before the weights shipped, still applies. It recommends recording the actual artefacts and checksums, reviewing the licence for the weights, code and bundled components, mapping infrastructure needs, defining a security boundary for prompts, tools and credentials, and testing on a private set of representative coding or agent tasks. It adds that a low active-parameter count “is not a hardware bill or a service-level commitment”, and that Apache 2.0 still requires a review of notices and distribution obligations.

## Open questions and what to watch next

Several questions remain open in the available sources:

- **Independent benchmarks.** Reflection’s results have not been reproduced by a third party in these sources. Hugging Face is explicitly inviting the community to benchmark Beam and share the results. Those results will be the first real test of Reflection’s claims.
- **Technical report and model card.** Reflection had promised documentation, a technical report, a model card and developer materials alongside the weights. Details on training data, safety testing and the context-length figures should be checked against those documents.
- **Red-teaming outcomes.** Beam was in final red-teaming and evaluation as of the October 5 announcement. What that testing found has not been described in these sources.
- **Hosting options.** Hosted inference currently appears through Baseten and Together on Hugging Face. Watch for confirmed support on major cloud platforms. For context on how cloud model catalogues are handling new frontier models, see Google Cloud adding OpenAI’s GPT-6 models to Model Garden.

Beam also arrives in a busy field of large open-weight models aimed at agents, alongside releases such as Tencent’s Hy4 preview. How it compares with those models on the same tasks, measured by someone other than the maker, will decide whether it becomes a default choice for self-hosted coding agents.

## Frequently asked questions

### Is Reflection Beam open source?

Beam's weights are released under the Apache 2.0 licence on Hugging Face, and it ships with an open-source Rust-based harness. That licence allows commercial use, but teams still need to review notices and any bundled components.

### How big is Beam and what hardware does it need?

Beam has 501 billion parameters, of which about 23 billion run per token. All experts stay in memory, so it needs the memory of a 501B model; the BF16 checkpoint is about 1 TB. FP8 checkpoints target Hopper and Ampere GPUs, and an NVFP4 version targets Nvidia Blackwell.

### What is Beam's context window?

Hugging Face's release post lists a 1M-token context window. An earlier analysis of Reflection's announcement cited a maximum context of 256,000 tokens during training, so long-context performance is worth testing directly.

### Where can developers run Beam?

The weights can be downloaded from Hugging Face and run with transformers, vLLM or SGLang. Reflection's Hugging Face listings also show hosted inference through Baseten and Together.

### Has Beam been independently benchmarked?

Not in the sources available. Reflection publishes its own results across coding, terminal, reasoning, tool-calling and search benchmarks, and Hugging Face is inviting the community to benchmark the model and share results.
