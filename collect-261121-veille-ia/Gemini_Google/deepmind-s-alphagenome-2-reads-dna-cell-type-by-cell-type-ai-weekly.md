---
id: collect-261121-veille-ia/gemini-google/deepmind-s-alphagenome-2-reads-dna-cell-type-by-cell-type-ai-weekly
title: "DeepMind's AlphaGenome 2 reads DNA cell type by cell type | AI Weekly"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "deepmind", "model", "type", "variants", "google", "gene"]
source: https://aiweekly.co/alerts/deepminds-alphagenome-2-reads-dna-cell-type-by-cell-type
source_anchor: ""
source_lines: [1, 21]
section: "Gemini_Google"
---
## TL;DR

- AlphaGenome 2 predicts how DNA variants affect gene expression in specific human cell types rather than broad tissue averages.
- On the TenK10K immune-cell benchmark, the model flagged 30% of causal variants against the first AlphaGenome's 16%.
- Non-commercial researchers get API access, commercial use is in private preview, and DeepMind says the model is not approved for clinical use.

Google DeepMind on November 11 released AlphaGenome 2, a successor to its DNA-interpretation model that predicts how genetic variants act inside specific human cell types rather than averaging across whole tissues. On the TenK10K benchmark covering 28 primary immune cell types, the DeepMind team reports the model flagged 30% of causal variants against the first AlphaGenome's 16%, and detected 43% of directional gene effects versus 23%.

The architectural shift behind those numbers is that AlphaGenome 2 no longer tries to predict thousands of experiments in parallel. It is a conditional model, trained on more than 100 million single-cell profiles across 68 tissues, that takes a cell type or biological condition as input and returns effect predictions for that context. DNA methylation prediction and cross-modality imputation of unmeasured properties like chromatin accessibility are new additions.

"What makes AlphaGenome 2 exciting is how it adapts directly to new single-cell data without retraining, predicting how DNA variants act in newly profiled cell states from even a small sample of cells," said Sarah Teichmann, Human Cell Atlas co-founder and professor of stem cell medicine at Cambridge, in the DeepMind post. The write-up also shows the model reproducing a known inflammatory bowel disease mechanism, in which a variant drives the condition by overactivating the ETS2 gene in gut macrophages, and distinguishing vaccine-activated T cells in COVID-19 analysis.

Peter Donnelly, CEO of Genomics Limited, called it "an impressive advance in the field." Non-commercial researchers get access through the AlphaGenome API; commercial use sits behind a private preview, with future distribution planned through Google's Antigravity agentic platform. DeepMind states the model is not approved for clinical or diagnostic use.

## Shared on Bluesky by 3 AI experts

          Originally reported by
          **deepmind.google**
        

Original headline: AlphaGenome 2: Deciphering how cells read DNA to better understand disease — Google DeepMind
