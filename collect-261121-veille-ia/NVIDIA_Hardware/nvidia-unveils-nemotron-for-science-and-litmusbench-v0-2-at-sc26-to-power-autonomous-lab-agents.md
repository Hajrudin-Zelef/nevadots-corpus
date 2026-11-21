---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-nemotron-for-science-and-litmusbench-v0-2-at-sc26-to-power-autonomous-lab-agents
title: "NVIDIA Unveils Nemotron for Science and LitmusBench v0.2 at SC26 to Power Autonomous Lab Agents"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "nemotron", "science", "litmusbench", "sc26", "open", "scientific", "agents"]
source: https://dailymech.com/articles/nvidia-unveils-nemotron-for-science-and-litmusbench-v0-2-at-sc26-to-power.html
source_anchor: ""
source_lines: [1, 69]
section: "NVIDIA_Hardware"
---
# NVIDIA Unveils Nemotron for Science and LitmusBench v0.2 at SC26 to Power Autonomous Lab Agents

Combining the 120B-parameter multimodal Nemotron 3.5 Super VL model with nearly 50,000 chemistry reasoning questions, NVIDIA gives researchers an open stack for local scientific discovery.

Published

Last updated / Fact-checked on

*Illustrative photo; it is not a picture of the companies, products, or places named in this article. blickpixel / Pixabay, Pixabay Content License.*

"NVIDIA"

## "Nemotron for Science"

"LitmusBench"

"SC26"

"Scientific AI" image: "https://pixabay.com/photos/ai-robot-artificial-intelligence-7977960/" image_source: "Pixabay"

On November 16, 2026, at the opening of the SC26 supercomputing conference in Chicago, NVIDIA introduced Nemotron for Science, an open artificial intelligence initiative built to accelerate autonomous agentic workflows across chemistry, materials science, and molecular biology. Rather than offering another closed cloud API, the release packages open model weights, an empirical chemistry dataset, and reinforcement-learning post-training recipes so laboratories can run specialized scientific agents on hardware they control—ranging from a desktop 128GB NVIDIA DGX Spark workstation to national supercomputers.

“Accelerated computing is transforming how scientists tackle the world's most complex challenges,” said Ian Buck, vice president of hyperscale and high-performance computing at NVIDIA, in the company's SC26 announcement. “Nemotron for Science gives researchers open models, scientific data and post-training recipes to build specialized agents that reason through scientific problems and use CUDA-X accelerated tools to turn ideas into discoveries.”

## Inside the Stack: Nemotron 3.5 Super VL and LitmusBench v0.2

As detailed in CASRAI's SC26 technical coverage and NVIDIA's release, Nemotron for Science brings together three core pillars, all immediately available on Hugging Face and NVIDIA NGC:

Nemotron 3.5 Super VL: A multimodal Mixture-of-Experts (MoE) vision-language foundation model with 120 billion total parameters and 12 billion active parameters per token. Built to process text, high-resolution scientific figures, chemical reaction schemes, and instrumentation video in a single unified pipeline, it avoids the context loss of chaining separate vision and text models. Using optimizations including LiLiCorr speculative decoding, NVFP4 quantization with distillation, multi-token prediction, and context reuse, the model delivers over 600 tokens per second on a single GPU on agentic benchmarks—up to 5x higher throughput than comparable open models.

LitmusBench v0.2: An open dataset of nearly 50,000 chemistry reasoning questions drawn from experimental drug discovery records and molecular property databases. It evaluates models on aqueous solubility, pharmacokinetic profiles, and absorption, distribution, metabolism, and excretion (ADME) pathways. Crucially, CASRAI reports that LitmusBench v0.2 is structured for Reinforcement Learning with Verifiable Rewards (RLVR), using empirical experimental outcomes as programmatic reward functions rather than subjective human preference labels.

NeMo Gym and NeMo RL Post-Training Recipes: Open workflows for supervised fine-tuning (SFT), low-rank adaptation (LoRA), and reinforcement learning. On drug-metabolism benchmarks, NVIDIA reported a 12% accuracy gain from SFT followed by an additional 17% gain from reinforcement learning under a 16,384-token budget.

## Why Open, Local Scientific Agents Matter

First-generation scientific AI breakthroughs like AlphaFold and ESMFold excelled at static structural prediction. The new wave of agentic systems goes further: reading literature diagrams, writing simulation scripts, debugging failed runs, and drafting wet-lab synthesis protocols.

However, academic and biopharma labs face practical barriers when relying on proprietary cloud models. Sending pre-patent molecular scaffolds or clinical datasets to external endpoints creates intellectual property and export-control risks, while iterative overnight agent loops can rack up unpredictable per-token API bills. By optimizing Nemotron 3.5 Super VL to run locally on a 128GB DGX Spark desktop as well as institutional clusters, NVIDIA lets research computing directors convert variable cloud operating fees into predictable on-premises infrastructure.

## Early Benchmarks Across Labs and Life Sciences

Several national laboratories and biotech companies published early production results alongside NVIDIA's SC26 launch:

Lila Sciences (Quantum-Dot Synthesis): Using post-trained Nemotron to generate execution-ready laboratory protocols with automated lab feedback, Lila Sciences nearly doubled accuracy on quantum-dot synthesis to 61%, outperforming GPT-6 Astra's 54% while cutting inference costs by 10x to 20x.

KTH Royal Institute of Technology (Simulation Debugging): Swedish researchers built an autonomous agent on Nemotron to diagnose and repair broken GROMACS molecular dynamics setups. Running overnight on a single 128GB DGX Spark system, the agent resolved 81% of broken test cases, with every fix independently verified by GROMACS.

Terray Therapeutics (Literature Extraction): By post-training Nemotron to parse reactants, products, and reaction conditions from chemical diagrams, Terray recovered 76% of annotated reactions on the RxnScribe benchmark.

Therna (mRNA Design): Collaborating with NVIDIA on agentic mRNA therapeutic design, Therna nearly doubled accuracy in predicting protein-expression ranges over baseline models, with research scientists preferring its mechanistic explanations in 96% of blind comparisons.

National Supercomputing Centers: Argonne National Laboratory is integrating Nemotron 3.5 Super VL into its StructBioReasoner multi-agent platform for protein therapeutic candidates; RIKEN deployed the models on its RIKYU AI supercomputing platform via the RiVault inference gateway; and Germany's Jülich Supercomputing Centre is adopting the stack for automated simulation steering.

## What Comes Next

With Nemotron 3.5 Super VL and LitmusBench v0.2 live on Hugging Face, NVIDIA and partners from Argonne and KTH are running live demonstrations of automated simulation debugging and chemical property prediction at SC26 in Chicago. Looking ahead, CASRAI notes that NVIDIA plans to expand NeMo Gym environments into additional scientific domains—including crystallography and genomics—as academic consortia begin independent evaluations across standard cheminformatics benchmarks.

## Sources

- Official Press Release: NVIDIA Introduces Nemotron for Science to Accelerate Discoveries With Agents — GlobeNewswire (Nov 16, 2026)
- Technical Analysis: NVIDIA Unveils Nemotron for Science and LitmusBench v0.2 at SC26 — CASRAI (Nov 16, 2026)
- Header Image: Pixabay — AI Robot Artificial Intelligence

## How this edition was merged

This story combines overlapping drafts prepared for DailyMech from the daily-mech GitHub repositories and the Claude and Gemini Drive folders. Where those drafts disagreed, the more specific figure is attributed to the source named in the copy. Figures that a draft already marked unverified stay unverified here.

*DailyMech used AI tools to help research and draft this article; it was fact-checked by our editors.*
