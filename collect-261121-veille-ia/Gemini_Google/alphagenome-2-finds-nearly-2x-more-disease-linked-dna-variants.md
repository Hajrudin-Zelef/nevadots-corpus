---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-finds-nearly-2x-more-disease-linked-dna-variants
title: "AlphaGenome 2 finds nearly 2x more disease-linked DNA variants"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "model", "variants", "cell", "genetic", "researchers", "cells", "types"]
source: https://interestingengineering.com/ai-robotics/deepmind-alphagenome-2-causal-dna-variants
source_anchor: ""
source_lines: [1, 33]
section: "Gemini_Google"
---
Google DeepMind has unveiled AlphaGenome 2, an upgraded DNA sequence model that predicts how genetic variants affect different human cell types, helping researchers investigate the genetic mechanisms underlying disease.

The model builds on the original AlphaGenome, which maps molecular activity across a broad range of tissues and covers both coding and non-coding regions of the genome. This provides a unified approach to assessing the potential effects of genetic variants.

Earlier this year, the company introduced the AlphaGenome Atlas, a separate predictive resource mapping the effects of every possible single-letter DNA change in the human genome.

## From tissues to cells

According to the company, AlphaGenome 2 moves beyond broad tissue-level averages to analyse individual cell types and states. This allows researchers to examine how genetic variants affect disease-relevant cells and how their effects may differ across cellular conditions.

The model was trained on more than 107 million individual cell profiles from human and mouse samples spanning 68 tissues. Unlike its predecessor, which generated predictions across multiple tissues in parallel, the updated model takes a specific biological condition alongside a DNA sequence as input.

Researchers can select a cell type from a reference catalogue or use their own single-cell data to examine a previously unseen cellular state. The model can also estimate missing experimental measurements, such as chromatin accessibility, from other available data.

## Nearly twice as many variants

In a benchmark using TenK10K, a single-cell dataset covering 28 primary immune cell types, AlphaGenome 2 identified 30% of causal variants, compared with 16% for the original model fine-tuned on single-cell data and other specialised tools, according to Google DeepMind.

The model also improved its ability to determine the direction of a variant’s effect on gene activity. It correctly identified whether a variant increased or decreased gene expression in 43% of cases, compared with 23% for the earlier model and comparison tools.

The results suggest the updated system could help researchers narrow down which genetic changes are most likely to influence disease-related processes in particular cell types.

## Testing disease-linked variants

Google DeepMind also demonstrated the model’s ability to examine genetic effects in cellular conditions it had not previously encountered.

In one example, researchers used single-cell data from a study of people receiving a COVID-19 mRNA vaccine. AlphaGenome 2 distinguished activated immune T cells from resting ones and found that a variant associated with severe COVID-19 had a stronger effect on gene activity in the activated cells.

The company also used the model to reproduce computationally a finding involving inflammatory bowel disease (IBD). The variant, located in a distant regulatory region of DNA, was shown to increase activity of the ETS2 gene in gut macrophages, a type of immune cell. The model predicted that this effect was concentrated in those cells rather than occurring broadly across other cell types.

These examples illustrate how cell-specific predictions could help researchers connect genetic variants with the cells and biological processes involved in disease. However, computational predictions still need experimental validation.

AlphaGenome 2 is available for non-commercial research through the AlphaGenome API, while commercial researchers can access it through a private preview. Google DeepMind said the model is intended for research use and has not been validated or approved for clinical or diagnostic purposes.
