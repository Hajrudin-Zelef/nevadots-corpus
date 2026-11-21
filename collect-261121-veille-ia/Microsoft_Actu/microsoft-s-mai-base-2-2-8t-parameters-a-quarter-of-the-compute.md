---
id: collect-261121-veille-ia/microsoft-actu/microsoft-s-mai-base-2-2-8t-parameters-a-quarter-of-the-compute
title: "Microsoft's MAI-Base-2: 2.8T Parameters, a Quarter of the Compute"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "model", "training", "compute", "mai-base-2", "parameters", "trillion", "every"]
source: https://www.intelligentliving.co/microsoft-mai-base-2-trillion-parameters/
source_anchor: ""
source_lines: [1, 77]
section: "Microsoft_Actu"
---
Microsoft has finished pre-training its largest base model to date. MAI-Base-2 is a mixture-of-experts system with 2.8 trillion total parameters, of which 80 billion are active for any given token. The company says it beats every open-weight model it tested, and that its training recipe is 4 to 12 times more compute-efficient than the one behind DeepSeek’s strongest open base model.

Its AI chief goes further, claiming the model matches OpenAI’s best on a quarter of the compute. That part has not been published. The technical report from October 15 covers the efficiency numbers, the training run and the data. The rest arrived secondhand.

MAI-Base-2 is a sparse model, meaning only a slice of its network runs for each token. Microsoft trained it on 16,000 Nvidia GB200 GPUs over 38 days in August and September 2026, on a mixture of roughly 50 trillion tokens.

Across the run, average model FLOPs utilization was 21%, and goodput, the share of time that training actually progressed, was 89%.

The model was also trained with vision from the start through an encoder-free architecture, rather than having image understanding bolted on afterwards. Its vision-language corpus holds 4.5 trillion unique tokens and, with repetition, made up 18% of the training mixture.

Microsoft puts the knowledge cutoff at March 2026 and says the overall corpus grew about 36% in unique tokens compared with MAI-Base-1.

The sparsity is the point. Only about 2.9% of the network fires for any given token, which is what makes a 2.8-trillion-parameter model trainable on a realistic GPU budget. IL has previously looked at why dense and mixture-of-experts designs behave so differently at the same nominal size.

Microsoft compares pre-trained models on bits per byte, a measure that is agnostic to each model’s tokenizer. For code, it used its own codebase. For STEM, math, and question answering, it commissioned evaluations from outside vendors to reduce the risk that the tests had leaked into training data, then decontaminated every evaluation set against its own corpus. The full methodology, including all four evaluation results, is set out in Microsoft’s technical report.

The reference point is DeepSeek-V4-Pro-Base, chosen because it is the only released base pre-training checkpoint above one trillion parameters. DeepSeek has been the benchmark others measure against all year, including in IL’s coverage of V4.1 Flash and its pricing. Kimi K3, Qwen3.8-2.4T-A95B, and MiMo-V2.6-Pro-RL are also included, with the caveat that only post-trained versions are available, and post-training can move those scores either way.

To separate the quality of its recipe from raw scale, Microsoft fit a scaling law of bits per byte against training FLOPs for each of four evaluations. It then estimated how much compute a model on its own scaling ladder would need to match DeepSeek. The answer: its recipe is 4.2 to 12.3 times more FLOP-efficient. Against MAI-Base-1, the improvement is 6.3 to 12.7 times.

Those figures come from Microsoft’s own fits, on its own ladder, with MAI-Base-2’s final score included in the fit. That does not make them wrong. It does mean nobody outside Microsoft has reproduced them yet.

The efficiency gains are not one clever trick. They come from a stack of infrastructure changes that mostly exist to stop a very sparse model from wasting time:

- **Mixture of experts in every layer.** MAI-Base-1 alternated dense and expert layers because the expert implementation was too inefficient. Fixing memory fragmentation from dynamic routing and reducing per-expert overhead lifted expert-layer utilization from 24% to 35%, which made experts in every layer worthwhile.
- **Dropless routing.** Dropping overflow tokens hurt performance, but allocating memory for the worst-case imbalance wasted it. Microsoft groups experts and runs repeated dispatch, compute, and collect rounds at a fixed per-expert capacity, letting overloaded experts borrow unused capacity from neighbors. Memory stays flat under any routing pattern.
- **The Muon optimizer.** Muon needs each full weight matrix to orthogonalize momentum, which conflicts with the sharding used to spread training across GPUs. Microsoft reshards momentum, orthogonalizes locally, then sends the updates back. After tuning, Muon accounted for 2.1% of average iteration time, about 165 milliseconds.
- **Activation gradient clipping.** Standard clipping scales down a whole batch when one outlier appears. Microsoft clips activations at selected points instead, so a single bad example does not shrink everyone else’s gradient. The benefit grew with scale, which is unusual, and it doubled as a data-quality detector: it flagged a training example where a license header was cut off mid-sentence, traced to a preprocessing bug that has since been fixed.

Microsoft’s claim is narrow but real: on its own held-out evaluations, MAI-Base-2 produced lower bits per byte than every model it compared against, including Kimi K3, Qwen3.8-2.4T-A95B, and MiMo-V2.6-Pro-RL. That is a meaningful result for a base model, because those scores reflect how well it predicts text before any instruction tuning or safety work.

What is missing is the kind of evidence most readers would want before trusting the headline. There are no scores on standard public benchmarks, no independent evaluation, and no release date.

MAI-Base-2 is still in post-training, the stage that turns a raw model into something usable. Microsoft says it plans to license the weights to partners through Foundry, but has not published terms or pricing.

| Detail | What Microsoft has said | Status | 
|---|---|---|
| Size | 2.8 trillion total parameters, 80 billion active | Published | 
| Training run | 16,000 GB200 GPUs, 38 days, about 50 trillion tokens | Published | 
| Efficiency vs DeepSeek | 4.2 to 12.3 times more FLOPs-efficient | Published, not independently replicated | 
| Performance vs open models | Lower bits per byte on four held-out evaluations | Internal evaluations only | 
| Comparison with GPT-5.6 | Stronger base model using a quarter of the compute | Claimed privately, no data published | 
| Release date and price | Weights to licensed partners through Foundry | Not announced | 

The “quarter of the compute” line comes from an internal town hall, where Microsoft AI chief executive Mustafa Suleyman reportedly said the company now has “a stronger pre-trained base model than the model that currently powers GPT 5.6 Sol,” according to Business Insider’s account of the meeting. He also described the model as using 12 times less computation than DeepSeek. Neither comparison appears in the technical report, and the 12-times figure sits at the top of Microsoft’s own 4.2 to 12.3 range, not in the middle of it. Treat both as claims until independent tests exist.

Owning the full pipeline lets Microsoft tune models and infrastructure together, then license the resulting weights to what it calls trusted partners through Foundry. That plan depends on clean data provenance. Microsoft says MAI-Base-2 was trained on organic data with no distillation from other models, which matters legally if you intend to sell access to the weights.

It is also a hedge. A company that can train a frontier-class base model at a fraction of the usual compute is less exposed to GPU scarcity and less dependent on any single partner’s roadmap. That theme runs through much of IL’s coverage of work aimed at squeezing more training out of fewer chips.

This is the part most coverage will skip. A recipe that is several times more efficient does not make inference several times cheaper, and it does not cut emissions by the same factor. Microsoft put the savings straight back into a bigger model. MAI-Base-2 has 2.8 trillion parameters precisely because the compute it saved made a larger run affordable.

That is the normal pattern. Efficiency gains get reinvested in capability, so total compute demand can keep rising even as compute per unit of capability falls.

The climate question then depends on deployment: how many tokens the model serves, where the servers sit, and how often it is called. Training is a one-time cost. Inference scales with usage, which is why data center power demand keeps showing up on grid operators’ balance sheets.

Microsoft’s own framing is that efficiency is how it keeps scaling at all. The company says MAI-Base-3 is already in preparation.

Whether that translates into lower prices or simply larger models is a business decision, not a technical one, and the report does not answer it. IL has looked at why Chinese labs have been able to push prices down faster than their American rivals, and part of that gap is exactly this choice about where savings go.

It is Microsoft’s newest pre-trained foundation model, the base that later products and fine-tuned versions are built on. Microsoft completed pre-training in October 2026 and is now running post-training.

It has 2.8 trillion total parameters, but only 80 billion are active for any given token because it uses a mixture-of-experts design. That works out to roughly 2.9% of the network running at once.

Microsoft has not shown that publicly. Its AI chief reportedly made the comparison in an internal meeting, but no benchmark scores, model card, or independent testing have been released. On Microsoft’s own held-out tests, it beats every open-weight model the company compared it against.

There is no release date. Microsoft says it will license the weights to trusted partners through Foundry once post-training is complete, which suggests access will be commercial and limited rather than a public download.

It is a design that splits the network into many specialized sub-networks, or experts, and routes each token to only a few of them. That keeps the compute per token far below what a dense model of the same total size would need.

- Public benchmark results for MAI-Base-2 once post-training finishes.
- Whether Foundry partners get access and at what price.
- Independent replication of the 4.2 to 12.3 times efficiency claim.
- Whether the quarter-of-the-compute comparison holds up under public evaluation.
- What MAI-Base-3 inherits from this run, and whether the efficiency curve keeps bending.

The disclosed numbers stand on their own. Training a 2.8-trillion-parameter model in 38 days at 21% utilization is a serious infrastructure achievement, and the dropless routing work is the kind of unglamorous engineering that makes sparse models practical. What is missing is public evidence that anyone outside Microsoft can check. Until that arrives, the efficiency figures are a strong internal result, not an established one.
