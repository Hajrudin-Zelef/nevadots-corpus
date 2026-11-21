---
id: collect-261121-veille-ia/muse/muse-spark-open-meta-s-open-ai-model-on-2-mac-studios-the-threshold
title: "Muse Spark Open: Meta's open AI model on 2 Mac Studios | The Threshold"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-15"]
keywords: ["meta", "open", "model", "muse", "apple", "spark", "studios", "macs"]
source: https://thethreshold.media/blog/muse-spark-open-meta-open-weights-mac-studio-2026
source_anchor: ""
source_lines: [1, 91]
section: "Muse"
---
# Muse Spark Open: Meta's open AI model on 2 Mac Studios

Meta released Muse Spark Open on October 15, 2026: the downloadable, open-weight version of its Muse Spark coding and agent model. Meta calls it the most capable US open-weight model to date. The headline trick is where it can run: Meta and Apple say it works entirely on your desk across two Mac Studio computers with the M5 Ultra chip, no cloud account needed.

Here is what Muse Spark Open is, what it costs to use through an API or to run at home, how fast it is on Macs according to Apple's own tests, where it beats rivals and where it doesn't, and what the license lets you do.

## What is Muse Spark Open?

Muse Spark Open is a large AI model that Meta lets anyone download. It has about 816 billion parameters in total, but it is a mixture-of-experts design: each layer has 256 small expert networks and only 8 of them are used for each word-piece (token), so about 42 billion parameters do the work at any moment. That keeps it fast, but the whole 816 billion still has to sit in memory.

It accepts text, images, audio and video, writes text back, and has a context window of 1,048,576 tokens (about a million), according to Meta's developer documentation. Meta says it is built for coding and agent work: long multi-step tasks, complex software engineering and tool calling. Hugging Face, which hosts the weights, adds that it can read a screenshot, listen to a voice note or watch a screen recording while it works on your code.

It ships in two formats. The full-precision BF16 version is about 1.65 TB of weights and needs a server with eight top-end GPUs such as Nvidia B300s. The compressed NVFP4 version stores most weights in 4-bit form, comes to about 537 GB, and runs on a single four-GPU Nvidia Blackwell machine, per Meta's docs.

## Can you really run it on two Mac Studios?

Yes, with the right Macs. Meta's hardware guide lists two supported Apple setups for the NVFP4 version using Apple's MLX framework: two Mac Studios with M5 Ultra and 512 GB of unified memory each, or four with 256 GB each. Every Mac needs macOS 26.2 or later, MLX 0.30.1 or later, a direct Thunderbolt 5 cable between every pair of Macs, and RDMA (a fast direct-memory link over Thunderbolt) switched on.

Meta's pitch is that two of these Macs act like one machine with a terabyte of memory. Apple says the M5 Ultra's GPU can use up to 512 GB per Mac Studio and has 1.2 TB/s of memory bandwidth, which is fast enough to read the small slice of the model each token needs. Meta says it tested real agent jobs fully on-device with its Muse Code tool, including shipping bug fixes and features to the open-source Mastodon app Ice Cubes and editing video end to end in DaVinci Resolve.

Apple's machine learning team published its own numbers the same day. At an 8,192-token context, Muse Spark Open generated 15.74 tokens per second on two Mac Studios splitting the model by layers (pipeline mode), 24.99 tokens per second on two Macs splitting each layer (tensor mode), and 36.73 tokens per second on four Macs in tensor mode. Time to the first token at that context was 8.4, 8.2 and 4.8 seconds respectively. Apple connected each pair of Macs with two Thunderbolt cables to double the link speed.

Apple says a Mac Studio with M5 Ultra peaks at about 300 watts during these model runs, so the setup can sit in a normal home or office. One honest caveat from Apple's post: in compressed 4-bit form the model takes about 493 GB, and with working memory on top it cannot fit on a single Mac.

## How much does it cost?

The model itself is free to download, but Hugging Face gates the files behind a short access form. The hardware is the real cost. Apple's store lists the Mac Studio with M5 Ultra from $5,499 (30-core CPU) and $6,799 (36-core CPU), checked October 15, 2026 at 21:24 UTC. Two machines therefore start at $10,998 before any memory upgrade, and the 512 GB memory option Meta's two-Mac setup needs isn't on sale yet: Apple says it arrives in late October and hasn't published its price.

If you just want to use the model, hosted APIs are far cheaper. OpenRouter lists Muse Spark Open at $1.25 per million input tokens and $4.25 per million output tokens, with cached input at $0.15 per million, served by Together, Nebius Token Factory, Fireworks and Baseten (checked 21:23 UTC October 15). Meta also names Groq and Modal as launch partners, and the weights are listed on Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry.

## Muse Spark Open benchmarks: where it leads and where it trails

Meta's published scorecard compares Muse Spark Open (at its highest reasoning setting) with other open-weight models, and these are Meta's numbers, not independent tests. It leads on several agent and knowledge tests: 61.8 on JobBench (GLM 5.3 scores 61.4), 89.2 on MCP-Atlas (Qwen3.8 scores 84.5), and 26.1 on the hard HLE-Diamond test without tools, ahead of Kimi K3 at 22.2.

Coding is the weak spot, which matters because Meta pitches it as a coding model. On Terminal-Bench 2.1 it scores 76.4, behind Kimi K3 (85.0), GLM 5.3 (83.9), Qwen3.8 (82.0) and Beam (80.1). On DeepSWE 1.1 it scores 60, behind Kimi K3 (68), GLM 5.3 (67) and Mistral Large 4 (62). GLM 5.3 also beats it on the GDPval-AA v2 office-work test, 1653 to 1401. So 'most capable US open-weight model' is a claim about American models; several Chinese open models still lead on coding.

Image understanding is a strength. With tools, it scores 50.0 on ZeroBench and 73.6 on SimpleVQA, ahead of Kimi K3 on both, while several rivals in the table don't accept images at all.

## Can you use Muse Spark Open commercially? The license explained

Mostly yes. The Muse Model License on Hugging Face lets you use, copy, modify, fine-tune, distill and redistribute the model for free. You must show 'Built with Muse Spark Open' in your product's interface, documentation, website or settings.

The main limit is for companies that sell AI models as an API service. If a company and its affiliates make more than $20 million in gross revenue in any 12-month period, it must tell Meta and sign a separate agreement before offering the model as that kind of service. The rule does not apply to internal use, research, or using the model to power features inside your own app.

Meta's usage policy also bans uses such as military and weapons work, running critical infrastructure, and malware, and says the model is not intended for anyone under 18. Meta says it claims no ownership of what the model generates.

## Why this matters

Meta built its AI reputation on the open Llama models, then moved its newest Muse Spark versions behind its own API and Muse Code plans. Muse Spark Open is its first big open release since the small Muse Glimmer model in August, and it arrives with day-one support from Nvidia, AMD, Intel and Apple hardware, plus popular tools such as vLLM, SGLang, llama.cpp and Unsloth.

For most people the useful way in is a hosted API at a few dollars per million tokens. The two-Mac setup is aimed at developers and companies that want a strong model on hardware they control, so code and documents never leave the building. Even then, 'local' only covers where the model runs: any tools or web searches an agent uses can still reach the internet.

Sources: Meta AI Research blog and Meta Model API docs (Muse Spark Open overview and hardware platforms), Apple Machine Learning Research post on MLX distributed inference, Hugging Face model card, license and launch blog, OpenRouter's Muse Spark Open page, Apple's Mac Studio newsroom release and store, and VentureBeat coverage. All checked October 15, 2026 between 21:21 and 21:27 UTC (02:51 to 02:57 IST October 16).

See more of The Threshold on Google

Add us as a preferred source to see more of our stories in your Top Stories.

## Frequently asked questions

## What is Muse Spark Open?

Muse Spark Open is Meta's open-weight AI model released on October 15, 2026. It has about 816 billion total parameters with about 42 billion active per token, accepts text, images, audio and video, and has a context window of about one million tokens.

## Can Muse Spark Open run on a Mac?

Yes, but not on one Mac. Meta's guide supports two Mac Studios with M5 Ultra and 512 GB of memory each, or four with 256 GB each, linked by Thunderbolt 5 and running macOS 26.2 or later with MLX 0.30.1 or later.

## How fast is Muse Spark Open on Mac Studio?

Apple measured 36.73 tokens per second on four M5 Ultra Mac Studios and 24.99 tokens per second on two (tensor mode) at an 8,192-token context. Two Macs in pipeline mode reached 15.74 tokens per second.

## How much does Muse Spark Open cost?

The weights are free to download under Meta's Muse Model License. Through OpenRouter it costs $1.25 per million input tokens and $4.25 per million output tokens (checked October 15, 2026). Running it locally needs at least two M5 Ultra Mac Studios, which start at $5,499 each before memory upgrades.

## Is Muse Spark Open free for commercial use?

For most uses, yes, with a 'Built with Muse Spark Open' credit. Companies over $20 million in yearly revenue that offer it as an AI model service need a separate agreement with Meta. Internal use, research and features inside your own product are exempt.

## How big is the Muse Spark Open download?

Meta says the full BF16 weights are about 1.65 TB and the 4-bit NVFP4 version is about 537 GB.

## Is Muse Spark Open better than Kimi K3 or GLM 5.3?

On Meta's own scorecard it leads on agent tests such as JobBench and MCP-Atlas and on image tasks, but trails Kimi K3 and GLM 5.3 on the coding tests Terminal-Bench 2.1 and DeepSWE 1.1.

## Where can I get Muse Spark Open?

The weights are on Hugging Face (meta-models), Amazon SageMaker AI, Google Cloud Model Garden and Microsoft Foundry. Hosted access is available through providers including Together AI, Fireworks, Baseten, Nebius, Groq, Modal and OpenRouter.
