---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-deepmind-teaches-ai-to-read-dna-cell-by-cell-to-hunt-for-disease
title: "AlphaGenome 2: DeepMind Teaches AI to Read DNA Cell by Cell to Hunt for Disease"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "model", "disease", "deepmind", "gene", "read", "cells"]
source: https://dev.to/rriostrujillo/alphagenome-2-deepmind-teaches-ai-to-read-dna-cell-by-cell-to-hunt-for-disease-1p6j
source_anchor: ""
source_lines: [1, 31]
section: "Gemini_Google"
---
— What it is: DeepMind launched AlphaGenome 2, a model that predicts how a DNA variant alters gene activity in a specific cell type, not in a tissue average.

— The leap: in the TenK10K test it identified 30% of causal variants, nearly twice as many as its predecessor.

— The limit: it is a research tool, not a clinical one: it is not validated for diagnosis or medical use.

One of Google's great successes was, first, to acquire DeepMind and, second, to let Demis Hassabis, the Nobel Prize-winning genius, manage its inventions. Today its products — AlphaFold, AlphaGenome and AlphaGenome 2 — represent a different point of view on the use of AIs: one focused on bringing down disease and public health. While much of the AI race pursues chatbots and generated video, DeepMind's genomics line is betting on a slower and more difficult prize: health.

AlphaGenome 2 starts from a simple fact: Almost every cell in the body carries the same DNA, but a heart muscle cell and an immune cell do completely different jobs. The difference isn't in the code, but in which parts of the code each cell reads and when. The model takes a DNA sequence of up to a million letters along with a condition—a tissue, a cell type, or a state, such as a vaccine-activated T cell—and predicts gene expression, RNA splicing, chromatin accessibility, histone marks, and DNA methylation for that particular condition.

Its predecessor, AlphaGenome, presented in June 2025 and published in Nature in January 2026, predicted molecular activity in many tissues at once. Powerful, but fuzzy: a variant that turns on a gene in a rare cell type was diluted in the tissue average. AlphaGenome 2 reconstructs the architecture around that idea: it's a conditional model that feeds the condition at each stage of the network, so that the same DNA sequence, read under different conditions, produces different predictions.

The training grew in tandem: more than 107 million profiles of individual cells in 68 tissues, with human and mouse data. And there is a detail that changes the daily work of researchers: the model can receive a sample from an experiment of its own from a single cell – a few dozen cells is enough – and use it as a condition without retraining, even for states it has never seen. It can also fill in missing measurements: with only the expression of the genes, it predicts unmeasured properties such as chromatin accessibility.

The numbers, read without frills: in TenK10K, a map of 28 types of immune cells, AlphaGenome 2 recovered 30% of the causal variants with 90% accuracy, compared to 16% in the original AlphaGenome adjusted for data from a single cell. It got the direction of the effect in the gene right 43% of the time, compared to 23%. And in regions of the genome excluded from the training, it explained a median of 34% of the variance in gene expression specific to each cell type, compared to 23%. The honest nuance is that 30% also means that seven out of ten causal variants escape it, and these are figures from the day of the launch, from the laboratory that built the model, without independent replication yet.

The most convincing evidence is not in the table but in two cases of disease. In inflammatory bowel disease, a study had shown that a variant in a distant regulator switch causes disease by overactivating the ETS2 gene in immune cells in the gut. AlphaGenome 2 reproduced that finding by calculation: from a set of seven candidate variants it targeted rs2836882, linked it to ETS2 about 290 kilobases away, and located the effect in monocytes and macrophages in the gut. According to James Lee, the immunologist who led the original study, the model arrived at that answer in about 30 minutes, compared to the year and a half it took for the original work. In the second case, using single-cell data from a COVID-19 mRNA vaccine study that the model did not know about, it distinguished vaccine-activated T cells from those at rest and predicted that a variant associated with severe COVID-19 alters gene activity more strongly in the activated state, an invisible effect on unactivated cells.

For Latin America, the announcement matters for an awkward reason: large genomic databanks are still dominated by populations of European descent, and the region is underrepresented. Tools like this can help finally close the findings of genomic association studies that have been unresolved for years, but only if Latin American genetic diversity is in the data. Mexico has the capacity to try, with the National Institute of Genomic Medicine (INMEGEN) and one of the most diverse populations in the world: the opportunity is to use these tools on Mexican and Latino data, and not just import foreign conclusions.

AlphaGenome 2 is now available for non-commercial research through the AlphaGenome API, with commercial access in private preview and tutorials in Colab. DeepMind also published pre-computed resources: links between enhancers and genes in 640 reference conditions and 39,323 genes, and scores for more than 6.8 million variants in 510 human conditions. DeepMind's head of genomics, Žiga Avsec, noted that predictions at the cellular level can help study drug targets, because drugs act on specific proteins in specific cell types. And the company is preparing AlphaGenome 2 Skills for Antigravity, its agent-based development platform, while Genomics PLC plans to incorporate the model into its Mystra platform.

What is NOT covered: there is no clinical use or diagnosis —DeepMind says it without nuance—; there is no replication independent of numbers; the model is heavier than its predecessor, because each condition demands its own prediction run, so an atlas of the entire genome with AlphaGenome 2 is not practical and the original model is still in service for wide scans; and its representations of cellular state come from observational profiles in a steady state, that is, they capture correlations and not causal mechanisms.

What's next: independent replication by other groups, the use of pre-calculated scores to close pending genomic findings, and integration with Antigravity, which will show whether DeepMind sees AlphaGenome 2 as a loose tool or as an instrument within a scientific flow with agents. DeepMind also receives use cases from the community until November 30, and selected projects will be able to receive extra quota and credits.

Sources and credits: Google DeepMind — AlphaGenome 2: Deciphering how cells read DNA to better understand disease | Dataconomy — Google DeepMind releases AlphaGenome 2 To Study Disease | AI Frontier Post — DeepMind launches AlphaGenome 2: DNA variant predictions, cell by cell | Unite.AI — Google DeepMind's AlphaGenome 2 Predicts Cell-State Variant Effects. Cover: AI Frontier Post, AI-generated editorial illustration.

Originally published in Digitalia.

## Top comments (0)
