---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-how-google-deepmind-taught-an-ai-to-read-dna-through-a-cell-s-eyes
title: "AlphaGenome 2: How Google DeepMind Taught an AI to Read DNA Through a Cell's Eyes"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "model", "condition", "variant", "deepmind", "sequence", "tissue"]
source: https://dev.to/prabhakar_chaudhary_7afe4/alphagenome-2-how-google-deepmind-taught-an-ai-to-read-dna-through-a-cells-eyes-4pbg
source_anchor: ""
source_lines: [1, 44]
section: "Gemini_Google"
---
For decades, one of the hardest problems in human genetics has been connecting a DNA variant to the specific cell type where it actually does something. Genome-wide association studies (GWAS) can flag thousands of variants linked to disease, but most of them sit in non-coding regions — the 98% of the genome that doesn't encode proteins — and their effects depend entirely on which cell is reading that stretch of DNA. A variant that disrupts gene regulation in a gut immune cell may be invisible in a liver cell.

Google DeepMind's AlphaGenome 2, announced on November 11, 2026, is a direct attempt to solve this problem. It's a conditional DNA sequence model that predicts how genetic variants affect molecular activity in specific human and mouse cell types — not just broad tissue averages. The results are striking enough that a finding which took a research team 18 months of wet-lab experiments was reproduced computationally in about 30 minutes.

The original AlphaGenome took a one-million-letter DNA sequence and predicted properties across thousands of experiments simultaneously — a parallel, multi-task approach. That design was efficient but spread the model's capacity thin. It couldn't distinguish between closely related cell types within the same tissue, and it couldn't tell you *where* a variant was acting.

AlphaGenome 2 flips this design. Instead of predicting everything at once, it's a **conditional model**: it takes a DNA sequence *and* a target biological condition — a specific cell type, tissue, or state — and predicts molecular activity for that one condition per call. The condition is injected at every stage of the architecture using FiLM (Feature-wise Linear Modulation) layers, which modulate the model's internal activations based on the cell-type embedding. DeepMind found that conditioning the encoder and transformer layers (not just the decoder) gave the biggest performance gains.

The architecture itself is a U-Net style design: a convolutional encoder, an 11-block transformer, and a decoder. The condition embedding is 256-dimensional and is applied throughout, allowing the model to learn how the same DNA sequence is interpreted differently depending on which cell is reading it.

The scale of training data is what makes the conditional approach viable. AlphaGenome 2 was trained on more than 107 million individual cell profiles across 68 tissues, covering both bulk and single-cell measurements from human and mouse samples. This is a substantial step beyond the bulk tissue averages that trained AlphaGenome 1.

The training ran in four stages: pre-training on bulk and pseudobulk tracks, condition refinement (tuning embeddings and FiLM layers), student-teacher distillation, and post-training the expression encoder. The model predicts the same core readouts as its predecessor — gene expression, histone modifications, chromatin accessibility, splicing — and adds DNA methylation, a frequently requested capability.

One of the more practically useful features is the **cell encoder**, which lets AlphaGenome 2 generalize to cell types and states it was never trained on. Researchers can provide a small single-cell RNA-seq snapshot — as few as a few dozen cells — and the model compresses that gene activity data into a 256-dimensional condition embedding. That embedding is then used to condition predictions for the new state, without any retraining.

This matters for disease research. A variant's effect on an immune cell during active inflammation may differ from its effect in a resting state. AlphaGenome 2 demonstrated this with COVID-19 vaccine data: given unseen single-cell data from a vaccine study, the model distinguished activated T cells from resting ones and identified that a variant linked to severe COVID-19 disrupts gene activity more strongly in the activated state — an effect that would be invisible in bulk tissue data.

The API documentation reflects this design: each call to `predict_sequence`, `predict_interval`, or `score_variant` takes one condition, either a reference condition from the catalog or a `cell_conditioning` input with raw UMI counts and Ensembl IDs (up to 256 cells per call).

The performance improvements are concrete. On the TenK10K dataset — a large single-cell map of 28 primary immune cell types — AlphaGenome 2 identified nearly twice as many causal variants as the original AlphaGenome fine-tuned on single-cell data:

- 
**Causal variant recall:** 30% vs. 16%
- 
**Directional gene effect accuracy:** 43% vs. 23%

The IBD example is the most illustrative. A 2024 study by James Lee's group at the Francis Crick Institute identified a variant in a gene desert that drives inflammatory bowel disease by overactivating *ETS2* in gut macrophages — a finding that required 18 months of gene-editing experiments. AlphaGenome 2 reproduced the key result computationally: it singled out the causal variant, identified gut macrophages as the relevant cell type, and correctly predicted the direction of the effect (upregulation of *ETS2*), while showing the variant had little effect in other cell types. As Lee told Nature News: "They've managed to figure out the answer in 30 minutes, and it took us a year and a half of experiments."

A secondary capability worth noting is cross-modality imputation. Because AlphaGenome 2 separates a cell's biological state from its predicted outputs, it can infer unmeasured properties from a single measurement. If a researcher has single-cell gene expression data but no chromatin accessibility data, the model can predict the missing readout. This is useful for prioritizing which experiments to run next, particularly in resource-constrained settings.

AlphaGenome 2 is available for non-commercial research through the AlphaGenome API, with interactive Colab tutorials for expression conditioning. Commercial research access is in private preview via Google Cloud. DeepMind has also announced AlphaGenome 2 Skills for Google Antigravity, their agentic development platform, pairing the model with Gemini 4 for complex genomic workflows.

The model is not a replacement for AlphaGenome 1 in all cases. The original remains useful for scoring variants across all tissues simultaneously without selecting a condition, and for reproducing results from the AlphaGenome 1 paper. AlphaGenome 2 is more computationally intensive per call, since it processes one condition at a time.

Neither model is validated for clinical or diagnostic use — both are research tools.

For researchers working at the intersection of human genetics and cell biology, AlphaGenome 2 addresses a real bottleneck: the gap between GWAS signals and functional understanding. Most GWAS variants sit in non-coding regions, and most functional assays are run in stable cell lines that don't reflect the specific cell types where disease actually unfolds.

A model that can predict variant effects in primary human cell types — including unseen disease states — and do so in minutes rather than months changes the economics of variant prioritization. It won't replace wet-lab validation, but it can substantially narrow the search space before experiments begin.

The conditional architecture is also a design pattern worth watching. The shift from parallel multi-task prediction to condition-specific inference, with FiLM-based modulation throughout the network, is a general approach that could apply to other domains where context determines how the same input should be interpreted.

## Top comments (0)
