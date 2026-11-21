---
id: collect-261121-veille-ia/modeles-open/reflection-s-beam-is-the-new-open-weight-model-everyone-is-talking-about
title: "Reflection's Beam Is the New Open-Weight Model Everyone Is Talking About"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "beam", "reflection", "token", "billion", "about", "open-weight", "parameters"]
source: https://towardsdatascience.com/reflections-beam-is-the-new-open-weight-model-everyone-is-talking-about/
source_anchor: ""
source_lines: [1, 63]
section: "Modeles_Open"
---
Here is a calculation anyone can do in under a minute. Beam, the new open-weight model from Reflection AI, has 501 billion parameters, and at 16 bits each parameter takes two bytes. That puts the weights at roughly 933 GiB, which lines up almost exactly with the published size of the BF16 checkpoint on Hugging Face.

The number most people repeat about Beam is different: 23 billion active parameters per token, and while that's real and useful, it doesn't shrink those 933 GiB by a single byte.

The attention is easy to understand. Beam is Reflection's first open-weight model, and the company positions it as a challenger to Chinese open models such as GLM-5.2. The launch was covered by Reuters and TechCrunch, and when the weights followed, the major inference frameworks had support ready the same day. That is a lot of excitement for a model whose headline numbers come mostly from its own authors.

Reflection announced Beam on October 5 and released the weights on October 16 under an open license, so this is a look at a model you can actually download, written after the first wave of headlines has passed. The question worth asking is what that 23 billion buys you, and what it leaves exactly where it was.

## How 501 billion parameters become 23 billion

Beam is a sparse mixture-of-experts model, which sounds more intimidating than the idea behind it. A dense model sends every token through every parameter, whereas a sparse one splits much of the network into smaller expert networks and lets a router pick a few of them for each token. Only the chosen experts do any work on that token, and that is where the 23 billion comes from.

The word I would underline in all of that is token. Active parameters tell you how much computation one token costs, which is why the number speaks to speed and price per token, but they say nothing about how many weights have to sit in memory.

The router can send the very next token to a completely different set of experts, so none of them can be left out. It is a bit like a hospital with hundreds of specialists, where each patient sees only a few, so every visit is cheap, but the building and the payroll still have to cover everyone.

That is the part of the design I find clever, and also the part the headline numbers skip over: the cost moves from compute to memory instead of going away, and memory is the bill you pay before the first token is even generated.

## What the efficiency claim actually says

Reflection reports that Beam matches GLM-5.2 on advanced reasoning benchmarks while using three to four times less inference compute. That claim comes from Reflection, and TechCrunch notes that the performance and efficiency claims remain independently unverified.

Reflection also acknowledges that Kimi K3 is ahead on raw capability, so the pitch rests on efficiency, and that makes it worth looking closely at what the efficiency claim actually measures.

It helps to notice what kind of claim this is. Three to four times less compute is an estimate built from how many parameters are active and how many tokens a model generates while it reasons. Both parts matter, since a model that thinks in fewer tokens costs less even at the same size. But an estimate of compute says little about measured latency, throughput, electricity, or what a provider charges you, because those depend on hardware and software that the estimate leaves out.

There is also a setting that can swing the bill on its own. Beam’s API defaults to medium reasoning effort, while OpenRouter lists max as the route default. The same prompt can cost very different amounts depending on which door you walk through.

## The deployment reality check

Reflection released Beam in three numeric formats. BF16 is the full precision release, FP8 stores each weight in 8 bits, and NVFP4 is NVIDIA’s 4-bit format for its Blackwell GPUs.

I went through Reflection’s release post along with the NVIDIA Dynamo and AWS deployment guides, so the table below puts the published sizes next to my own arithmetic.

| Format | Published size | Plain multiplication | 
|---|---|---|
| BF16 | 933.81 GiB | 933.2 GiB | 
| FP8 | 468.72 GiB | 466.6 GiB | 
| NVFP4 | 273 GiB | 233.3 GiB | 

The sizes are worth a second look. BF16 and FP8 land within about half a percent of the plain multiplication, which is what I expected. NVFP4 is the odd one out, coming in roughly 40 GiB heavier than 4 bits per parameter would suggest. I guess that some of that is scaling data and some layers kept at higher precision, but that is only a guess, and the model card is the place to confirm it.

The lesson is that quantization shrinks the weights without being free, and that quality and speed are not guaranteed to match across formats. For scale, the guides I read put FP8 on eight H200 GPUs and NVFP4 on four B300s, so even the smaller build is a multi-GPU server.

If you want to run the check yourself, it takes a few lines of Python.

Weights are also only part of the memory story. The runtime adds its own overhead, and the KV cache grows with every token of context, which matters for a model that advertises a window of up to one million tokens.

Then there is the gap between open and easy, because Apache 2.0 settles the license question without saying anything about the hardware one. Beam has day-zero support in Transformers, vLLM, and SGLang, but the framework and the kernels you pick will shape real throughput, and nothing in a parameter count tells you that.

## Who should care, and what is still uncertain

Mixture-of-experts makes a large model cheaper to compute, and Beam shows that, though it does nothing to make the model small. Whether it pays off for you depends on your hardware, your serving setup, your workload, and the quality you actually get, none of which a headline number can settle.

For most people, a hosted API is the sensible first test. The teams that need to read the hardware requirements closely are the ones that must keep their data in-house, and for them the answer is a data center-class server, not a workstation. Independent evaluations are still thin, so treat the benchmark numbers as Reflection’s own claims until others reproduce them.

So when the next model arrives with a big total and a small active count, do the multiplication first. The number that matters is the one that tells you what you have to store.

## **Before you go!**

I write about AI and ML engineering, and about what new model releases mean for the people who have to build with them. You can subscribe to my newsletter if you would like more of that.

**Connect With Me**
