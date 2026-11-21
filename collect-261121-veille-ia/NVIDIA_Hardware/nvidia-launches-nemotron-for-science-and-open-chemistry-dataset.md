---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-launches-nemotron-for-science-and-open-chemistry-dataset
title: "NVIDIA Launches Nemotron for Science and Open Chemistry Dataset"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "chemistry", "model", "nemotron", "scientific", "dataset", "data", "design"]
source: https://www.unite.ai/nvidia-launches-nemotron-for-science-and-open-chemistry-dataset/
source_anchor: ""
source_lines: [1, 45]
section: "NVIDIA_Hardware"
---
## Key PointsVerification is the package's core design because experimental references and domain…

View more

- Verification is the package's core design because experimental references and domain software score the model's outputs.
- LitmusBench turns measured experimental outcomes into automated rewards that remove the need for human preference labeling.
- NVIDIA's reported gains cover bounded tasks and do not establish general laboratory autonomy.

NVIDIA introduced Nemotron for Science on November 16, 2026, pairing its open Nemotron 3.5 Super VL model with scientific evaluations, post-training recipes and LitmusBench v0.2, a chemistry dataset containing nearly 50,000 questions for training and evaluating research agents.

The package is intended for agents that interpret scientific information, generate code and call computational tools, including CUDA-X accelerated software. Researchers can post-train the model with domain-specific data for work such as chemistry experiments and protein design, adapting a general vision-language system to a particular scientific task.

Its model card dates the underlying model’s Hugging Face release to October 15, 2026, and describes 120 billion total parameters, 12 billion active parameters, text, image and video inputs, text outputs and a context window of up to one million tokens. The scientific data, evaluations and customization recipes are therefore the new substance of this announcement.

The model and dataset are available on Hugging Face, and NVIDIA has optimized the model to run locally on a 128GB DGX Spark system.

## Chemistry Data and Verifiable Training

LitmusBench v0.2 draws on molecular properties and experimental drug-discovery data, covering behavior such as solubility and how the human body processes molecules. The dataset repository identifies English text-generation data tagged for chemistry, molecular-property prediction and reinforcement learning with verifiable rewards, and lists CC-BY-SA-4.0 and OpenMDW-1.1 licenses.

An NVIDIA technical guide explains the accompanying recipe. Experimental CSV records become question-answer pairs, with their measured outcomes serving as an automated verifier. That creates a reinforcement-learning environment without human preference labeling; cheminformatics tools, molecular simulators or a live autonomous laboratory can supply the scoring layer.

In the guide’s drug-metabolism example, the model receives a parent molecule and an enzyme, then predicts the resulting metabolite. A zero-to-one Tanimoto score measures overlap between the predicted and reference molecular fingerprints, providing a reward based on structural similarity. NVIDIA reports that supervised fine-tuning improved accuracy by 12% in this example and reinforcement learning added a further 17% at a 16,384-token output budget.

An optional warm-start uses a stronger teacher to generate reasoning traces and retains those that meet the similarity threshold before reinforcement learning; the example uses Anthropic’s Claude Sonnet 5.5.

## Task-Level Results in Life Sciences

The announced applications span structured chemical extraction, laboratory-protocol generation and therapeutic design. These NVIDIA-reported results concern bounded tasks and do not establish general laboratory autonomy.

Terray Therapeutics post-trained Nemotron to extract reactants, products and reaction conditions from scientific diagrams. NVIDIA reports that it recovered about 76% of annotated reactions on the RxnScribe test under that benchmark’s matching criteria, helping turn published chemistry into structured data for synthesis planning and scientific agents.

Lila Sciences used Nemotron to design execution-ready protocols for its automated laboratories. According to NVIDIA, post-training with laboratory feedback nearly doubled accuracy for making quantum dots — nanoscale crystals used in screens and solar panels — to 61%, compared with GPT-6 Astra’s 54%, at 10–20 times lower inference cost.

Therna collaborated with NVIDIA on agentic mRNA therapeutic design. According to NVIDIA, the tuned model achieved nearly twice the accuracy of frontier models in predicting protein-expression ranges, and scientists preferred its explanations to the base model’s in 96% of comparisons.

## Research Deployments and Local Infrastructure

Argonne National Laboratory is integrating Nemotron into StructBioReasoner, a multi-agent system for designing and evaluating protein-based therapeutic candidates for cancer and other diseases. Jülich Supercomputing Centre is also integrating the model into scientific-discovery agents.

KTH Royal Institute of Technology built a Nemotron agent to repair broken GROMACS molecular-dynamics setups. NVIDIA reports that it fixed 81% of test cases with reasoning enabled during one overnight run on a single DGX Spark, with every fix independently checked by GROMACS.

RIKEN has made Nemotron models, including 3.5 Super VL, available on its RIKYU AI supercomputing platform through the RiVault inference gateway for scientific research and accelerated-application development. Alongside local DGX Spark deployments, that gives research organizations a route to run scientific workflows on infrastructure they control.

The unifying design is verification: the open model supplies reasoning and tool use, while experimental references, laboratory feedback or domain software score its outputs. That makes task selection, the quality of the scoring method and held-out evaluation central to the package’s usefulness: a benchmark gain matters only when the verifier measures the scientific outcome researchers actually want.
