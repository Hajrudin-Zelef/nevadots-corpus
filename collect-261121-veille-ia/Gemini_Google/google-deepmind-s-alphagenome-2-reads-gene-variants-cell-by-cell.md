---
id: collect-261121-veille-ia/gemini-google/google-deepmind-s-alphagenome-2-reads-gene-variants-cell-by-cell
title: "Google DeepMind's AlphaGenome 2 reads gene variants cell by cell"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "gene", "alphagenome", "deepmind", "model", "variants", "google", "types"]
source: https://cor.ax/links/91106
source_anchor: ""
source_lines: [1, 13]
section: "Gemini_Google"
---
### Images

Google DeepMind has released AlphaGenome 2, a model that predicts how individual genetic variants behave in specific human cell types rather than as averages across whole tissues. Trained on more than 107 million single-cell profiles from 68 tissues in humans and mice, it identified nearly twice as many relevant gene variants as its predecessor in testing. It is available for noncommercial research.

Every cell in the body carries the same DNA; what differs is which genes each cell type switches on. A heart muscle cell reads the genome differently than an immune cell, so a mutation that silences a gene in one can leave the other untouched. Most assessments of whether a variant is harmful are still made against bulk tissue averages, and the first AlphaGenome worked the same way: it took a DNA sequence a million letters long and predicted its properties across thousands of experiments spanning many tissues at once. DeepMind says that parallel approach spread the model's capacity too thin to capture subtle differences between closely related cell types.

AlphaGenome 2 is a conditional model instead. It takes a DNA sequence plus a specific biological state and calculates effects for that one cell type: how strongly a gene is expressed, how tightly the DNA is packaged in the nucleus, and which chemical tags silence or activate genes. It can infer missing data from a single measurement — from gene activity alone it estimates how accessible stretches of DNA are — which DeepMind says helps researchers decide which lab experiments are worth running next.

On the TenK10K benchmark, which covers 28 primary immune cell types, the new model identified about 30 percent of causal variants and correctly predicted whether a variant turns a gene up or down in 43 percent of cases. A fine-tuned version of its predecessor managed 16 and 23 percent. The model also tracks continuous processes, such as blood stem cells maturing into red blood cells, with considerably greater accuracy than before.

The money sits upstream of the clinic. A variant's effect on a specific cell type is the raw material of drug target selection, and target selection is where drug programs commit their spending. A model that ranks those effects before any bench work begins is a tool for not spending on the wrong targets.

Working purely computationally, AlphaGenome 2 reproduced an inflammatory bowel disease mechanism from a published study. The variant responsible sits in a regulatory switch far from the gene it disrupts, selectively overactivating ETS2 in gut macrophages while other cell types and genes stayed largely unaffected — the sort of difference a tissue average cannot see.
