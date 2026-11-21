---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-packages-nemotron-model-and-chemistry-data-for-scientific-agents
title: "Nvidia packages Nemotron model and chemistry data for scientific agents"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "model", "nemotron", "chemistry", "post-training", "scientific", "agents", "questions"]
source: https://www.aiaffairs.com/technology/nvidia-nemotron-science-chemistry-data-post-training/
source_anchor: ""
source_lines: [1, 32]
section: "NVIDIA_Hardware"
---
Nvidia introduced Nemotron for Science on 16 November 2026, packaging its Nemotron 3.5 Super VL open model with a chemistry dataset, scientific evaluations and post-training recipes for building specialised research agents. The announcement came at the SC26 conference in Chicago, StreetInsider reported.

## Key points

- LitmusBench v0.2 contains nearly 50,000 questions for training and evaluating chemistry reasoning.
- Nvidia says Terray Therapeutics recovered about 76% of annotated reactions on the RxnScribe test after post-training Nemotron.
- Nvidia reports that Lila Sciences reached 61% accuracy on a quantum-dot synthesis task after training with laboratory feedback.
- A Nvidia technical recipe uses experimental results to score a model’s answers during post-training.

## LitmusBench v0.2 supplies chemistry questions

LitmusBench v0.2 is an open dataset of nearly 50,000 questions drawn from molecular properties and experimental drug-discovery data. Nvidia says researchers can use it both to evaluate chemistry reasoning and to adapt language models for tasks involving molecular behaviour, including solubility and how the body processes a compound. A common set of questions gives builders a way to test a specialised model on chemistry problems, rather than treating its general conversational ability as a substitute for that test.

The package builds on an existing model: AI Affairs previously reported the release of Super VL alongside two local Bolt models. Nvidia describes the scientific release as a way to adapt Super VL with task-specific data, then put it into agents that interpret research material, generate code and call computational tools. For a group building such a system, the model, evaluation questions and training instructions serve different purposes: the questions test or supply examples, while post-training changes the model’s responses to a specialised task.

## Nvidia’s recipe scores predicted molecules

A Nvidia technical guide sets out one post-training example using records of experimentally observed drug metabolism. It gives the model a starting molecule and an enzyme, then asks for the molecule produced by their interaction. Experimental records provide the answer against which the prediction is scored. Nvidia uses NeMo Gym to build that training environment and NeMo RL to update the model from the scores.

The guide represents molecules as strings describing their atoms and bonds, then compares structural features of the predicted and recorded products. Think of a fingerprint made from a molecule’s features: the score rises as more of the predicted fingerprint overlaps with the recorded one. That supplies feedback from experimental data without a person grading each answer. In Nvidia’s drug-metabolism example, at a 16,384-token output budget, supervised fine-tuning improved accuracy by 12% and reinforcement-learning post-training raised it a further 17%, according to the guide.

Reading a published chemical reaction could become a route to information usable in synthesis planning if its ingredients, products and conditions were recovered accurately from the diagram. That is the task Nvidia says Terray Therapeutics trained Nemotron to perform, and the resulting information could be organised for later research work rather than remaining inside an image.

On the RxnScribe test, Terray’s post-trained model recovered about 76% of annotated reactions under the benchmark’s matching criteria, Nvidia says. The company also says Terray surpassed previously published results on two benchmarks. Those measurements concern extraction from scientific diagrams, a different task from the drug-metabolism prediction used to illustrate Nvidia’s training recipe.

## Lila Sciences and KTH test specialised agents

Lila Sciences used Nemotron to design protocols for its automated laboratories, according to Nvidia. After post-training with laboratory feedback, accuracy on a task involving the making of quantum dots — tiny crystals used in screens and solar panels — nearly doubled to 61%, Nvidia says. The company compares that result with 54% for GPT-6 Astra and reports inference costs 10-20x lower.

Nvidia says Therna also post-trained Nemotron, in this case for mRNA therapeutic design, and nearly doubled accuracy in predicting ranges of protein expression against frontier models. In comparisons with the base model, scientists preferred the post-trained model’s explanations 96% of the time, the company reports. Argonne National Laboratory, meanwhile, is integrating Nemotron into StructBioReasoner, a system of multiple agents for designing and evaluating protein-based therapeutic candidates.

At KTH Royal Institute of Technology, an agent built on Nemotron repaired 81% of broken GROMACS molecular-dynamics test cases during one overnight run on a single DGX Spark system, Nvidia says. GROMACS checked each repair. RIKEN has made Nemotron, including Super VL, available through its RiVault inference gateway on the RIKYU AI supercomputing platform, according to Nvidia. Nemotron 3.5 Super VL and LitmusBench v0.2 are available on Hugging Face, and Nvidia says the model is optimised to run locally on a 128GB DGX Spark system, StreetInsider reported.
