---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-for-science-ai-chemistry-agents
title: "NVIDIA Nemotron for Science: AI Chemistry Agents"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "nemotron", "chemistry", "model", "scientific", "agents", "science", "data"]
source: https://businessbearings.com/articles/nvidia-ships-nemotron-for-science-with-50-000-question-chemistry-dataset
source_anchor: ""
source_lines: [1, 49]
section: "NVIDIA_Hardware"
---
By Amara Osei

4 min read

## What's News

- NVIDIA launched Nemotron for Science on November 16, 2026 with LitmusBench v0.2, a chemistry dataset of nearly 50,000 questions.
- The underlying Nemotron 3.5 Super VL model has 120 billion total parameters, 12 billion active, and a one-million-token context window.
- NVIDIA reports supervised fine-tuning lifted drug-metabolism accuracy 12%, with reinforcement learning adding another 17%.
- Lila Sciences reached 61% quantum-dot protocol accuracy versus GPT-6 Astra's 54%, at 10–20 times lower inference cost.
- A KTH-built Nemotron agent fixed 81% of broken GROMACS test cases in one overnight run on a single DGX Spark.

NVIDIA released Nemotron for Science on November 16, 2026, pairing its open Nemotron 3.5 Super VL model with scientific evaluations, post-training recipes and LitmusBench v0.2 — a chemistry dataset of nearly 50,000 questions for training and evaluating research agents.

The package targets agents that interpret scientific information, generate code and call computational tools, including CUDA-X accelerated software. Researchers can post-train the model on domain-specific data for work such as chemistry experiments and protein design, adapting a general vision-language system to a particular scientific task.

## What's actually new here?

The model itself isn't. Its Hugging Face release dates to October 15, 2026, per the model card, which describes 120 billion total parameters, 12 billion active parameters, text, image and video inputs, text outputs and a context window of up to one million tokens. The scientific data, evaluations and customization recipes are the new substance of the announcement. NVIDIA has also optimized the model to run locally on a 128GB DGX Spark system.

## How does verifiable-reward training work?

Verification sits at the package's core. LitmusBench v0.2 draws on molecular properties and experimental drug-discovery data, covering behavior such as solubility and how the human body processes molecules. The dataset repository identifies English text-generation data tagged for chemistry, molecular-property prediction and reinforcement learning with verifiable rewards, under CC-BY-SA-4.0 and OpenMDW-1.1 licenses.

An NVIDIA technical guide explains the recipe. Experimental CSV records become question-answer pairs, with measured outcomes serving as an automated verifier. That creates a reinforcement-learning environment without human preference labeling; cheminformatics tools, molecular simulators or a live autonomous laboratory can supply the scoring layer.

In the guide's drug-metabolism example, the model receives a parent molecule and an enzyme, then predicts the resulting metabolite. A zero-to-one Tanimoto score measures overlap between the predicted and reference molecular fingerprints, providing a reward based on structural similarity. NVIDIA reports supervised fine-tuning improved accuracy by 12% in this example, and reinforcement learning added a further 17% at a 16,384-token output budget.

An optional warm-start uses a stronger teacher to generate reasoning traces and retains those that meet the similarity threshold before reinforcement learning. The example uses Anthropic's Claude Sonnet 5.5.

## What did the life-science customers achieve?

The announced applications span structured chemical extraction, laboratory-protocol generation and therapeutic design. NVIDIA reports the results itself, and they cover bounded tasks only — they don't establish general laboratory autonomy.

- **Terray Therapeutics** post-trained Nemotron to extract reactants, products and reaction conditions from scientific diagrams. NVIDIA says it recovered about 76% of annotated reactions on the RxnScribe test under that benchmark's matching criteria, turning published chemistry into structured data for synthesis planning and scientific agents.
- **Lila Sciences** used Nemotron to design execution-ready protocols for its automated laboratories. Post-training with laboratory feedback nearly doubled accuracy for making quantum dots — nanoscale crystals used in screens and solar panels — to 61%, versus GPT-6 Astra's 54%, at 10–20 times lower inference cost.
- **Therna** collaborated with NVIDIA on agentic mRNA therapeutic design. The tuned model achieved nearly twice the accuracy of frontier models in predicting protein-expression ranges, and scientists preferred its explanations to the base model's in 96% of comparisons.

## Who's deploying it — and where does it run?

Argonne National Laboratory is integrating Nemotron into StructBioReasoner, a multi-agent system for designing and evaluating protein-based therapeutic candidates for cancer and other diseases. Jülich Supercomputing Centre is also integrating the model into scientific-discovery agents.

KTH Royal Institute of Technology built a Nemotron agent to repair broken GROMACS molecular-dynamics setups. NVIDIA reports it fixed 81% of test cases with reasoning enabled during one overnight run on a single DGX Spark, with every fix independently checked by GROMACS.

RIKEN has made Nemotron models, including 3.5 Super VL, available on its RIKYU AI supercomputing platform through the RiVault inference gateway for scientific research and accelerated-application development. Alongside local DGX Spark deployments, that gives research organizations a route to run scientific workflows on infrastructure they control.

The unifying design is verification: the open model supplies reasoning and tool use, while experimental references, laboratory feedback or domain software score its outputs. A benchmark gain matters only when the verifier measures the scientific outcome researchers actually want — so task selection, scoring quality and held-out evaluation will decide whether this package earns a place in real labs.

Source: unite.ai
