---
id: collect-261121-veille-ia/microsoft-actu/mai-base-2-pretraining-complete-four-innovations-behind-microsoft-s-412-efficiency-claim
title: "MAI-Base-2 Pretraining Complete: Four Innovations Behind Microsoft's 4–12× Efficiency Claim"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-10-15"]
keywords: ["model", "microsoft", "mai-base-2", "efficiency", "compute", "training", "claim", "deepseek-v4-pro-base"]
source: https://www.techtimes.com/articles/329192/20261019/mai-base-2-pretraining-complete-four-innovations-behind-microsofts-4-12x-efficiency-claim.htm
source_anchor: ""
source_lines: [1, 90]
section: "Microsoft_Actu"
---
Microsoft AI finished pretraining its MAI-Base-2 foundation model last month — a 2.8-trillion-parameter Mixture-of-Experts vision-language system trained on 16,000 NVIDIA GB200 GPUs over 38 days — and on October 15, 2026, published the engineering details behind a claim that drew immediate attention: the model achieved the equivalent of 4.2 to 12.3 times better FLOPs efficiency than DeepSeek-V4-Pro-Base. Four specific algorithmic and infrastructure choices drove those gains. But the efficiency figure requires careful reading: it is a scaling-law projection, not a direct compute comparison, and the MAI-Base-2 training run actually consumed more total compute than DeepSeek spent — which is why understanding the methodology matters as much as the number.

Post-training is ongoing. No public weights have been released, and no pricing or general availability date has been set for enterprise access through Microsoft Foundry. Preparations for a third-generation model, MAI-Base-3, are already underway.

Microsoft AI CEO Mustafa Suleyman posted on X on October 15, citing a 12× efficiency gain over DeepSeek-V4-Pro-Base; Microsoft's technical write-up gives the full range as 4.2× to 12.3×. The number measures something different from what it may initially appear to measure.

Microsoft did not train MAI-Base-2 with a fraction of DeepSeek's compute budget and arrive at the same performance. The actual MAI-Base-2 training run consumed more total compute than DeepSeek-V4-Pro-Base spent. Microsoft's blog explicitly states the team used the compute savings from the more efficient recipe to train a larger model. The efficiency figure comes from fitting scaling laws—empirical curves that predict how a model's benchmark performance improves as training compute scales up. Microsoft estimated how much compute a model trained with its recipe would need to match DeepSeek-V4-Pro-Base's benchmark scores, then compared that estimated crossover compute to what DeepSeek actually spent. The resulting ratio represents how much less compute Microsoft's recipe would theoretically require to match DeepSeek's scores — not how much less was actually used.

### What MAI-Base-2 Is

MAI-Base-2 is a Mixture-of-Experts sparse architecture in which a router function sends each token to a small subset of "expert" sub-networks rather than activating the full model weights. With 2.8 trillion total parameters and approximately 80 billion active parameters per token, only about 3% of the model's weights participate in any single inference pass. That sparsity makes computation tractable while allowing the model to store vastly more learned patterns than a dense architecture of equivalent active size would permit.

The training corpus was 50 trillion tokens, with 18% — approximately nine trillion tokens, including 4.5 trillion unique tokens — consisting of vision-language data. The vision set spanned image captioning from the web, chart understanding, visual grounding, spatial reasoning, medical image analysis, computer-use data, and sheet-music reading. A March 2026 knowledge cutoff applies. The training run achieved an average model FLOPs utilization (MFU) of 21% and a goodput — the fraction of time training was actually progressing rather than recovering from hardware failures or restarts — of 89%, measured against the NVIDIA GB200's theoretical BF16 peak of 2.5 petaFLOPS.

### Efficiency Claim: What the 4.2×–12.3× Ratio Measures

The wide range reflects domain variation: on some benchmark domains the efficiency advantage is more modest (4.2×); on others it is far larger (12.3×). The comparison models include DeepSeek-V4-Pro-Base, Kimi K3, Qwen3.8-2.4T-A95B, and MiMo-V2.6-Pro-RL. Microsoft evaluates base model quality using bits-per-byte (BPB) — a tokenizer-agnostic metric that measures how precisely a model predicts text, enabling fair comparison across models with different tokenization schemes. BPB scores reflect base model quality, not end-user task performance; post-training benchmark results on coding, agents, and mathematical reasoning have not yet been published.

The announcement does not disclose a second layer of uncertainty. The 4.2×–12.3× range spans domain variation, but the scaling-law fit itself carries uncertainty. Scaling-law researchers have documented that different fitting methodologies produce systematically different results: the Kaplan et al. (2020) paper and the Hoffmann et al. "Chinchilla" (2022) paper yield different optimal compute allocations, and the discrepancy traces to how parameter count is measured. The crossover-compute estimate inherits the uncertainty of its fitted scaling law — a range built on a statistical fit, whose confidence intervals Microsoft has not published.

### Muon Optimizer: Replacing AdamW at 16,000-GPU Scale

The first algorithmic change was the optimizer. MAI-Base-2 training uses the Muon optimizer in place of AdamW, the de facto standard for large language model training. AdamW preconditions gradients using a diagonal estimate of curvature — one scalar per weight, which is efficient but approximate. Muon instead orthogonalizes each gradient momentum matrix at each 2D weight by computing its nearest semi-orthogonal approximation through Newton–Schulz iterations — a numerical method for computing the matrix square root. The result is an update direction that is more uniformly scaled across a weight matrix's row and column space. Independent research found Muon provides a 2× compute efficiency gain over AdamW at moderate scales; Shah et al. (2025) found it outperforms AdamW across wide batch sizes.

Scaling Muon to 16,000 GPUs required engineering work not documented in prior literature. ZeRO sharding — the technique that partitions optimizer state across GPU memory to make training tractable at large scale — requires each parameter shard to be owned by one GPU for gradient accumulation and update steps. Muon's orthogonalization step, however, requires access to the full weight matrix, not a shard. The team resolved this with back-to-back all-to-all collective operations: before Newton–Schulz computation, momentum shards are redistributed so each GPU owns a contiguous block of rows; after computing locally, a second all-to-all returns each updated shard to its original owner. After these engineering changes, Muon added only 2.1% to average iteration time — 165 milliseconds per training step, against a theoretical lower bound estimated at around 55 milliseconds.

### Dropless MoE: Dispatch Without Discarding Tokens

Standard MoE training involves a routing problem: when too many tokens are assigned to the same expert in a single batch, the system must either discard overflow tokens or pre-allocate the memory required to handle the worst-case assignment in every batch. Dropping tokens degrades model quality; worst-case pre-allocation wastes GPU memory on every non-overflow batch.

Microsoft's solution is a multi-round dispatch system. Expert groups process tokens in one or more passes within a fixed, configurable memory footprint. Tokens that overflow an expert's capacity in the first round are deferred and processed in subsequent rounds rather than discarded. In steady state — when token distribution is well-behaved — a single round handles the full batch and the additional rounds add no overhead. Communication and computation across rounds are pipelined, and backward-pass recomputation is performed per expert group on a matching schedule.

The same engineering work fixed the memory-fragmentation problem that had constrained full-MoE training in MAI-Base-1, lifting MoE layer FLOPs utilization from 24% to 35%. That 46% improvement in productive compute use was enough to make all-MoE-layer training cost-effective for the full 38-day run. MAI-Base-1 alternated dense feed-forward layers with MoE layers—a common compromise to keep wall-clock training time manageable. MAI-Base-2 uses MoE in every layer.

### Activation Gradient Clipping: Surgical Stability That Scales

Training instability — gradient spikes that push the model into degenerate states and require costly restarts — is a persistent problem at frontier scale. The standard mitigation is global gradient norm clipping: if the norm of all gradients across the model exceeds a threshold, every gradient is scaled down proportionally. The problem is that a single batch outlier can trigger the clip and penalize well-behaved parameters, slowing training unnecessarily.

Microsoft instead clips gradients at specific activation sites within the network — individual locations in the computation graph where gradient magnitudes tend to spike — rather than applying a single global threshold. Two findings distinguished this approach. First, the stability gains from activation gradient clipping increased as model scale increased — unlike most modifications the team tested, which showed roughly constant returns regardless of model size. Second, the clipped activation sites functioned as data-quality sensors: when a spike appeared at a monitored location, the team could trace it back to a specific malformed training example. One such trace revealed a bug in code-data preprocessing that would otherwise have silently degraded training quality.

### Source Scaling: Configuration Advice That Transfers to Full Scale

Frontier-scale training runs are preceded by scaling ladders: small experiments that test configurations at reduced size to predict which choices will work best at full scale. A subtle bias corrupts this process. At small scale, the optimal configuration may perform well when data samples are seen many times—but at full scale, where the model sees each sample fewer times, a different configuration would win. Configurations tuned for small-scale sample diversity produce unreliable advice for the full run.

Microsoft's fix is source scaling to constrain sample diversity at each ladder rung, keeping the ratio of unique tokens to model parameters fixed across scales. This forces the small-scale experiments to mirror the data-repetition dynamics of the full run, making the small-scale winner genuinely predictive of the full-scale winner. The team also derived empirical scaling laws for learning rate, weight decay, and dropout jointly — noting that model performance is most sensitive to the product of learning rate and weight decay, not each independently — and used those laws to accurately predict MAI-Base-2's benchmark behavior before the full run began.

### Why Encoder-Free Vision Pretraining Differs From Bolt-On Approaches

MAI-Base-2's vision capability is not added after pretraining. There is no separately trained image encoder whose output is projected into the language model's embedding space — the architecture used by most production vision-language models. Instead, image data was incorporated throughout pretraining via an encoder-free architecture in which visual and language representations are learned jointly from the start.

The architectural difference carries real tradeoffs. Research on encoder-free vision-language models — notably the EVEv2 study from Beijing Academy of Artificial Intelligence — found the approach offers genuine advantages: because no pre-trained encoder imposes inductive biases about spatial layout, aspect ratio, or semantic organization, the model learns visual representations without those constraints. Lossless image encoding is another benefit: discrete visual tokenizers that compress images before feeding them to a language model lose information at the tokenization step; encoder-free models avoid that loss.

The tradeoffs include training instability and higher data requirements, since learning visual perception from scratch demands more high-quality vision data than fine-tuning a pre-trained encoder. The EVEv2 research found encoder-free models still trail encoder-based models overall, but the gap narrows significantly with more training data — positioning MAI-Base-2's 9-trillion-token vision corpus as a material advantage over prior encoder-free work.

### From MAI-Base-1 to Frontier in One Generation

MAI-Thinking-1, Microsoft's public reasoning model announced in June 2026, was post-trained on MAI-Base-1: a roughly one-trillion-parameter MoE trained on 8,192 GB200 GPUs with a 30-trillion-token corpus and no native vision pretraining. Between that model and MAI-Base-2, the unique training corpus grew by approximately 67%, the GPU count nearly doubled, and the total parameter count nearly tripled. Microsoft estimates the MAI-Base-2 recipe is 6.3×–12.7× more FLOPs-efficient than the recipe that produced MAI-Base-1 — suggesting that Muon, dropless MoE dispatch, activation gradient clipping, and source scaling compounded substantially within a single model generation.

### Enterprise Access Through Foundry

Microsoft is positioning MAI-Base-2 as an enterprise offering, not an open-weight release. The company explicitly cites control over the full model pipeline — data provenance, training from first principles without distillation from other models — as what enables it to license model weights to enterprise customers with a traceable, auditable training history. Distribution will be through Microsoft Foundry, the platform already hosting MAI voice and transcription services for enterprise customers.

No general availability date, partner list, or pricing has been announced. Post-training is ongoing, and the benchmark picture for practical enterprise tasks — coding performance, agent capability, mathematical reasoning, and multimodal workloads including document analysis with embedded charts and figures — will only become clear once post-training completes and results are published.

**Read more:**
**Microsoft Ships First Streaming Transcription Model, Completing Voice Agent Pipeline on Foundry**

### What Comes Next

Microsoft AI's blog noted that MAI-Base-3 preparation is already underway as MAI-Base-2 undergoes post-training. The three stated principles guiding the program — intelligence learned from first principles, simple methods that scale, and scientific rigor over shortcuts — suggest the team intends to continue compounding its architectural and infrastructure improvements. Suleyman, who has publicly called for human oversight of increasingly capable AI systems, has framed Microsoft AI's approach as one where frontier performance and responsible development reinforce rather than conflict with each other.

For enterprise AI buyers, MAI-Base-2's significance will hinge on what post-training produces. A 2.8-trillion-parameter base that outperforms available open-weight frontier models on perplexity-style evaluations is a meaningful research milestone. Enterprises accessing the model through Foundry will care most about practical performance on the vision-language tasks the architecture was designed for: processing complex documents with embedded charts, spatial reasoning over images, screen-based computer use, and the multimodal document analysis that currently drives demand for enterprise AI products. Those results are still pending.

## Frequently Asked Questions

### What does "4–12× more efficient" mean for MAI-Base-2, and why does the range vary so much?

The efficiency ratio is a scaling-law projection, not a direct compute comparison. Microsoft fitted empirical curves predicting how model quality improves with training compute, then estimated how much compute its recipe would theoretically need to match DeepSeek-V4-Pro-Base's benchmark scores. The 4.2×–12.3× range reflects domain variation: on some benchmark domains the efficiency advantage is modest (4.2×); on others it is substantially larger (12.3×). Importantly, MAI-Base-2's actual training run used more total compute than DeepSeek spent — Microsoft applied the efficiency gains to training a larger model, not to spending less. A second layer of uncertainty, which Microsoft did not disclose, is that the scaling-law fit itself carries statistical uncertainty; different fitting methodologies produce different crossover-compute estimates, and Microsoft has not published confidence intervals for its fitted curves.

### What is the Muon optimizer and why does it matter for large-model training?

Muon replaces AdamW by orthogonalizing each 2D weight's gradient momentum matrix — computing the nearest semi-orthogonal approximation via Newton–Schulz iterations — rather than using AdamW's simpler diagonal per-weight scaling. Independent research suggests Muon provides the equivalent of roughly a 2× compute multiplier over AdamW at moderate scales. Scaling it to 16,000 GPUs required novel custom sharding not documented in prior literature — specifically, back-to-back all-to-all collective operations to handle the conflict between ZeRO parameter sharding and Muon's need for whole weight matrices during orthogonalization. After that engineering work, the overhead was just 165 milliseconds per training step, or 2.1% of average iteration time.

### When will MAI-Base-2 be available for enterprise use through Microsoft Foundry?

No general availability date or pricing has been announced as of October 2026. Post-training is ongoing, and practical performance benchmarks on coding, mathematical reasoning, agent capability, and multimodal tasks will not be published until post-training completes. Distribution will be through Microsoft Foundry, the same platform that already hosts MAI voice and transcription models for enterprise customers.

### Why does encoder-free vision pretraining matter, and does it outperform existing approaches?

An encoder-free architecture means image understanding was woven into MAI-Base-2's pretraining from the start, rather than added via a separately trained image encoder. Advantages include lossless image encoding — no discrete tokenizer compresses the image before the model processes it — and no inductive biases from a pre-trained encoder imposing assumptions about spatial layout or aspect ratio. Current research finds encoder-free models still trail encoder-based models in aggregate performance at comparable data volumes, but the gap narrows significantly with more training data. MAI-Base-2's 9-trillion-token vision corpus represents a material data advantage over prior encoder-free work, making it difficult to separate architectural gains from data-volume gains until post-training results are published.
