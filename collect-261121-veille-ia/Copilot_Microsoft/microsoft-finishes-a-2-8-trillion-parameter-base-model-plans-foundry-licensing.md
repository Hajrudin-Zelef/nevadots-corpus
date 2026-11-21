---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-finishes-a-2-8-trillion-parameter-base-model-plans-foundry-licensing
title: "Microsoft Finishes a 2.8-Trillion-Parameter Base Model, Plans Foundry Licensing"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "foundry", "mai-base-2", "base", "post-training", "pre-training", "weights"]
source: https://dev.to/neticslabs/microsoft-finishes-a-28-trillion-parameter-base-model-plans-foundry-licensing-4fb9
source_anchor: ""
source_lines: [1, 56]
section: "Copilot_Microsoft"
---
- MAI-Base-2 is a foundation model from Microsoft AI: a mixture-of-experts network with 2.8 trillion total parameters, of which 80 billion activate for each token it processes. A base model is the pre-trained layer that later becomes a product after post-training, and Microsoft announced on 15 October 2026 that MAI-Base-2 had finished that pre-training stage.
- The run used 16,000 Nvidia GB200 GPUs over 38 days in August and September 2026, with an average model FLOPs utilization of 21% and goodput of 89%, meaning training progressed for 89% of the wall-clock time.
- Microsoft reports that its training recipe is 4.2 to 12.3 times more FLOPs-efficient than DeepSeek-V4-Pro-Base, and 6.3 to 12.7 times more efficient than its own MAI-Base-1.
- Those efficiency figures come from scaling laws fitted per evaluation, and Microsoft states that MAI-Base-2's own final score is included in the fits, which a reader should weigh before quoting the range.
- Three of the four open models Microsoft compares against are post-trained checkpoints; only DeepSeek-V4-Pro-Base is a released base checkpoint, and Microsoft flags that post-training may affect their scores.
- For a team that rents inference, three parts transfer: the active-parameter count that sets serving cost, the stated denominator behind an MFU figure, and the contract attached to weights offered through Foundry.
- The data story is the licensing argument: organic training data with no distillation from other models, a corpus about 36% larger than MAI-Base-1 in unique tokens, native vision with an encoder-free architecture, and a March 2026 knowledge cutoff.
- Distribution is a licence, and the licence is the news: the weights go to trusted partners through Foundry after post-training, with no open-weights release, no price and no date published.

Page 1 of Microsoft AI's own Lab Notes PDF on frontier pre-training (microsoft.ai, retrieved 2026-10-17), dated 15 October 2026, with the Figure 1 bits-per-byte table for models above one trillion parameters.

Microsoft AI published a Lab Notes post on 15 October 2026 announcing that pre-training of MAI-Base-2 is complete and that the model has moved into post-training. The post describes an MoE foundation model with 80 billion active and 2.8 trillion total parameters, and it states that MAI-Base-2 outperforms every open-weight model Microsoft tested on its held-out evaluations.

Two words in that description carry most of the story. Pre-training is the stage where a model learns from a large corpus and becomes a base checkpoint, and post-training is the stage that turns the checkpoint into something a user can address. A finished pre-training run is therefore an engineering milestone with a product still ahead of it, and Microsoft published no release date, no price per token and no statement about which of its own products the model will power.

The distribution statement is the one with commercial weight. Microsoft wrote that owning its model pipeline end to end "will enable us to license our model weights to trusted partners through Foundry", and that it tunes its models and infrastructure together "first in our own products and then for Foundry customers". Weights licensed to partners are a different product from weights published for download, and the post names no partners, no terms and no geography.

Netics visual: the four numbers Microsoft published for the run, with total and active parameters beside the utilization measurements.

The run itself is documented with unusual precision for a vendor announcement. Microsoft trained MAI-Base-2 on 16k GB200 GPUs over 38 days in August and September 2026. Over the run, the average MFU was 21% and the goodput was 89%. The post also states its own measurement convention: MFU is normalized by 2.5 PFLOPS, which it calls GB200's theoretical maximum in BF16, and the figure excludes FLOPs spent on rematerialization and on the optimizer.

Those two numbers describe different failures. MFU measures how much of the hardware's arithmetic capacity the kernels actually use, and Microsoft computes its 21% under a definition that excludes rematerialization and optimizer work. Our reading is that most of the gap to the theoretical peak is communication and memory time across 16,000 GPUs; the post does not break it down. Goodput measures the fraction of wall-clock time in which training progressed, so 89% describes a cluster that stalled for roughly one hour in nine. A team planning its own run should read both before comparing its own utilization figures against a vendor's.

The efficiency work behind the run is described in the same post, and one detail is directly reusable: fixing two inefficiencies in the MoE implementation raised the MFU of the MoE layers from 24% to 35%. Memory fragmentation from dynamic routing and per-expert overhead from finer-grained experts were the two causes named.

Netics visual: what the 4.2 to 12.3 figure compares, and the fitted scaling law it comes from.

The headline claim is that the recipe is 4-12x more pre-training FLOPs-efficient than DeepSeek-V4-Pro-Base, which Microsoft calls the strongest open base model. The post then explains how the number was produced, and the explanation matters as much as the number. Microsoft fitted an independent scaling law of bits-per-byte against training FLOPs for each of four held-out evaluations, estimated how much compute a model on its own ladder would need to match DeepSeek's bits-per-byte, and compared that estimate with DeepSeek's training compute. The result is 4.2 to 12.3 times, plus 6.3 to 12.7 times against MAI-Base-1.

One sentence in that method deserves quoting in full, because it bounds the claim: "MAI-Base-2's final performance is known, and we include it in the fits, so the matching point falls within the range of our fitted models rather than beyond it." An estimate anchored partly on the result it explains is an estimate, and Microsoft presents it as one. Third-party replication is what would turn it into a measured fact.

The comparison set has a second caveat, stated by Microsoft itself. Only DeepSeek-V4-Pro-Base is a released base pretraining checkpoint; Kimi K3, Qwen3.8-2.4T-A95B and MiMo-V2.6-Pro-RL are reported as post-trained checkpoints, and the post says their results carry the caveat that post-training may affect those evaluations. A reader who takes the table as a clean ranking of base models has read past that line.

Page 5 of Microsoft AI's Lab Notes PDF (microsoft.ai, retrieved 2026-10-17), carrying the Figure 3 scaling ladders used to estimate compute-equivalence against DeepSeek-V4-Pro-Base.

Microsoft ties its data position to its commercial one. The post says pre-training is built on organic data which "does not involve distillation from other models", and that every training token was processed in-house. The post ties the plan to license weights to partners to Microsoft owning that pipeline end to end. Provenance is a legal argument as much as a technical one: a lab that can show where its tokens came from has an easier time granting rights over the result.

The data volumes are concrete. The corpus grew roughly 36% from MAI-Base-1 to MAI-Base-2 measured in unique training tokens, the run used a 50 trillion token training mixture, and web data carries a March 2026 knowledge cutoff. Unlike its predecessor, MAI-Base-2 was pre-trained natively with vision through an encoder-free architecture, and its VLM corpus holds 4.5 trillion unique tokens, which with repetition accounted for 18% of the training mixture.

That vision share is the part most likely to matter to a buyer. Our reading: a base model with native image and document understanding needs less bolted-on machinery for chart, table and scanned-document work.

Nobody reading this will train a 2.8-trillion-parameter model, and three things in the post transfer anyway.

Page 12 of Microsoft AI's Lab Notes PDF (microsoft.ai, retrieved 2026-10-17), with the Figure 10 chart behind the statement that the corpus grew about 36% in unique training tokens.

Plan compute and memory from two different numbers. The 80 billion active parameters set the arithmetic, and so the cost, of each token. The 2.8 trillion total set the memory: every expert has to stay resident, so hosting the model needs room for all of them. A capacity plan built on only one of the two figures gets either the GPU count or the cost per token wrong.

Utilization figures need their definition attached. Microsoft publishes 21% MFU with the normalization and the exclusions written next to it, which is the standard to hold any vendor to. A utilization number without a stated denominator describes a slide.

A licensed base model is a supply-chain dependency with a contract attached. Weights delivered to trusted partners through Foundry come with terms a legal team has to read, and a business that builds a product on top of one should know whether it can keep running the model if the commercial relationship changes. Teams that need that control end up running weights they hold themselves; Netics assembles a self-hosted AI agent platform around infrastructure a customer owns for exactly that reason.

Microsoft says preparations for MAI-Base-3 are underway while MAI-Base-2 goes through post-training. The next announcement will probably carry a bigger number. The useful question stays the same: which of these figures can a customer check.

*Originally published on the Netics blog.*

## Top comments (0)
