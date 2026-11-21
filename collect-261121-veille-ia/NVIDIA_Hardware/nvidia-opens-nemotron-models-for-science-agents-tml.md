---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-opens-nemotron-models-for-science-agents-tml
title: "NVIDIA Opens Nemotron Models for Science Agents — TML"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "model", "nemotron", "science", "agents", "open", "models", "dataset"]
source: https://tml.news/stories/nvidia-nemotron-for-science-agents/
source_anchor: ""
source_lines: [1, 29]
section: "NVIDIA_Hardware"
---
# NVIDIA Opens Nemotron Models for Science Agents

NVIDIA introduced Nemotron for Science at SC26 in Chicago on Monday, November 16, 2026, packaging its open Nemotron 3.5 Super VL model with a new open chemistry dataset of nearly 50,000 questions and post-training recipes for building specialized research agents. The model and the dataset are available now on Hugging Face.

Edited and fact-checked by Stephen Sprague

The release is a toolkit, not a result. NVIDIA has handed researchers an open model, a chemistry test set and the recipes to specialize both — what that produces in real laboratories is the part no launch announcement can settle.

NVIDIA introduced Nemotron for Science on Monday, November 16, 2026, at the SC26 supercomputing conference in Chicago. The package brings together scientific applications of the company's open Nemotron 3.5 Super VL model, a new open chemistry dataset and post-training recipes, all aimed at helping researchers build specialized agents for scientific discovery — software that reasons through a research problem and calls specialized tools to work on it.

The model at the center is multimodal. Nemotron 3.5 Super VL takes text, image and video inputs and produces text, with 120 billion total parameters and 12 billion active parameters, according to its model card, and a context window of up to one million tokens. NVIDIA said it's optimized to run locally on a 128GB DGX Spark system, its desktop AI computer, which puts the model within reach of a single laboratory workstation instead of only a data center.

The dataset is the part built for checking the work. LitmusBench v0.2 is an open dataset of nearly 50,000 questions for training and evaluating language models on chemistry reasoning, drawn from molecular properties and experimental drug-discovery data, including behavior such as solubility and how the human body processes molecules. A benchmark with published questions gives outside researchers a way to test NVIDIA's claims instead of taking them on faith.

NVIDIA named the first users. Life sciences companies Lila Sciences, Terray Therapeutics and Therna are using and customizing the model for science applications. The U.S. Department of Energy's Argonne National Laboratory is integrating it into StructBioReasoner, a multi-agent system for designing and evaluating protein-based therapeutic candidates for cancer and other diseases. The Jülich Supercomputing Centre in Germany is integrating the model into its own scientific-discovery agents, and Japan's RIKEN has made Nemotron models available on its RIKYU AI platform.

The target fields are four: chemistry, life sciences, protein design and materials science. Those are areas where the work is already computational — screening molecules, predicting protein behavior, searching material properties — so an agent that can reason over the literature and then call simulation and analysis tools has something concrete to do. NVIDIA's pitch is that a general model specialized with the right data and recipes outperforms a general model left alone.

The honest limit sits in the results themselves. The performance figures NVIDIA published concern bounded tasks — defined questions with checkable answers — and they don't establish that these agents can run a laboratory program on their own. That's the standard gap between a benchmark and a discovery, and an open release is precisely how the gap gets measured: the model and LitmusBench v0.2 are both on Hugging Face now, where anyone can run them.

Distribution is the strategy. By giving away the model, the dataset and the recipes, NVIDIA makes its hardware and software stack the default workbench for a generation of science agents, including on the DGX Spark sitting on a researcher's desk. Whether Nemotron for Science produces discoveries is a question for the laboratories now downloading it. That it will be tried, widely and in public, is the one outcome the open release already guarantees.

## The conversation.

Loading comments…

## A little more perspective.

Reading is free. Verified members can comment and reply for $2 a month.
