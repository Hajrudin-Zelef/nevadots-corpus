---
id: collect-261121-veille-ia/gemini-google/google-deepmind-alphagenome-2-cell-type-dna-variant-predictions-benchmarks-and-api-access
title: "Google DeepMind AlphaGenome 2: cell-type DNA variant predictions, benchmarks and API access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "model", "single-cell", "google", "deepmind", "variant", "predictions"]
source: https://datastudios.org/post/google-deepmind-alphagenome-2-cell-type-dna-model-api-non-commercial-research
source_anchor: ""
source_lines: [1, 267]
section: "Gemini_Google"
---
Google DeepMind launches AlphaGenome 2 for cell-type DNA variant predictions

Nov 12

9 min read

Google DeepMind released AlphaGenome 2 on November 11, 2026, a new version of its DNA sequence model that predicts how a stretch of DNA, and any variant inside it, changes molecular activity in one specific human cell type or cell state at a time. The model is available from launch through the AlphaGenome API for non-commercial research, together with Colab tutorials, while commercial research access starts in private preview.

The change from the first AlphaGenome is architectural. The original model, released in June 2025, read up to one million DNA letters and produced a fixed set of parallel predictions for bulk tissues and cell lines. AlphaGenome 2 takes the same one-million-letter sequence plus a target condition, such as a tissue, a cell type or a cell state, and feeds that condition into every stage of the network. Google DeepMind says this lets the model learn how identical DNA is read differently by, for example, a gut macrophage and a T cell.

The headline claim is that AlphaGenome 2 identifies nearly twice as many expression-altering variants as the previous state-of-the-art model. On the TenK10K single-cell immune dataset, covering 28 primary immune cell types, it recovered causal variants in 30% of cases against 16% for a single-cell fine-tuned version of AlphaGenome 1, and predicted the direction of the effect in 43% of cases against 23%.

Training drew on bulk and single-cell data from human and mouse samples, with more than 100 million single-cell profiles across 68 tissues (the announcement also cites a figure of 107 million). The model adds DNA methylation to the outputs of the first version, which already covered gene expression, histone modifications and chromatin accessibility. Google DeepMind states that both AlphaGenome versions are for research use only and are not validated or approved for clinical or diagnostic use.

··········


WHAT ALPHAGENOME 2 PREDICTS AND HOW THE CONDITIONAL DESIGN WORKS.

Every cell in the body carries the same genome, yet a neuron, a liver cell and an immune cell switch on very different sets of genes. Large single-cell atlases such as the Human Cell Atlas now measure gene activity in millions of individual cells, but predicting how a single-letter change in DNA alters that activity in a particular cell type has remained an open problem. Most disease-associated variants found by genome-wide association studies sit outside protein-coding genes, in regulatory regions whose effects depend heavily on cell type.

AlphaGenome 2 addresses this by treating the cell condition as an input. According to the technical paper, the model receives a 1-megabase DNA sequence, a condition and a species identifier for human or mouse. Its backbone remains a U-Net-style network similar to AlphaGenome 1, and adaptive normalization layers modulate the sequence representations with a condition embedding from the earliest layers onward. The authors report that conditioning inside the transformer and encoder blocks produced most of the performance gains.

Conditions can come from a reference catalog of tissues and cell types, or from a user's own single-cell measurements. A separate cell encoder, inspired by the scVI single-cell autoencoder and trained on over 100 million human single-cell RNA sequencing profiles, compresses those measurements into a cell-state representation that the sequence model can use.

Because the biological state is separated from the outputs, a single type of measurement can be used to predict readouts that were never measured for that sample. Google DeepMind calls this cross-modality imputation: single-cell gene activity can be used to estimate chromatin accessibility or histone marks for the same cell state.

The model predicts RNA coverage, CAGE signal, chromatin accessibility and CpG methylation at single base-pair resolution, histone marks and transcription factor binding at 128 base pairs, and DNA contact maps at 2,048 base pairs. Splicing and gene-level expression are also among the modeled outputs.

··········

ALPHAGENOME 2 VS ALPHAGENOME 1: ARCHITECTURE, DATA AND OUTPUTS.

The table below sets the two versions side by side, using only the figures Google DeepMind published in its announcements and in the AlphaGenome 2 paper.

........

Feature

AlphaGenome 1

AlphaGenome 2

Release

June 25, 2025

November 11, 2026

DNA input

Up to 1 million base pairs

1 million base pairs

Prediction mode

Fixed parallel outputs for tissues and cell lines

One condition at a time (tissue, cell type or state)

Custom cell states

Not supported

From user single-cell data, no retraining

DNA methylation output

No

Yes, CpG methylation at 1 bp

Training focus

Bulk tissue and cell-line data

Bulk data plus over 100 million single-cell profiles, 68 tissues

Species

Human and mouse

Human and mouse

API access

Non-commercial research

Non-commercial research; commercial in private preview

........

The training corpus extends the bulk data used for AlphaGenome 1, drawn from sources such as ENCODE, GTEx and recount3. The paper lists 3,977 added single-cell RNA and single-cell ATAC pseudobulk tracks from 36 human and mouse studies, and 190 CpG methylation profiles across 185 human reference conditions. Single-cell post-training used human data from the CELLxGENE collection.

Training proceeded in four stages: pre-training, condition refinement, distillation of eight teacher models into a single student, and post-training. The pre-training stage ran for 100,000 steps on 2,048 TPU v4 chips, and the model is implemented in JAX and Haiku. Google DeepMind has not disclosed a parameter count.

··········

ALPHAGENOME 2 BENCHMARK RESULTS AGAINST ALPHAGENOME 1, BORZOI AND SCOOBY.

The paper compares AlphaGenome 2 with AlphaGenome 1, a single-cell fine-tuned AlphaGenome 1 (labeled AG1-sc), and external models including Borzoi, Scooby, CREsted, Avocado, ENCODE-rE2G and EGrf. The results below are reported by Google DeepMind and have not yet been reproduced independently.

........

Benchmark (metric)

AlphaGenome 2

Best previous result

GTEx tissue expression change (median Pearson r)

0.71

0.61 (AlphaGenome 1)

Single-cell expression variance explained

34%

23% (AG1-sc)

Single-cell accessibility variance explained

60%

51% (AG1-sc)

GTEx eQTL causality (AUPRC)

0.74

0.72 (AlphaGenome 1)

GTEx eQTL effect sign (AUC)

0.85

0.83 (AlphaGenome 1)

TenK10K causal variants (recall)

30%

16% (AG1-sc)

TenK10K effect sign (recall)

43%

23% (AG1-sc)

IBDverse effect sign (AUC)

0.86

0.81 (AG1-sc)

Distal enhancer-gene links, 100-2,500 kb (AUPRC)

0.62

0.48 (AlphaGenome 1)

........

The recall figures are measured at a fixed high-precision threshold of 90%, which explains why they look modest in absolute terms: they count how many true variants the model can flag while staying right nine times out of ten. On the GTEx eQTL causality task, recall at that threshold rose from 17% to 26%. Wherever the paper reports Borzoi alongside AlphaGenome 1, the margin over Borzoi is the wider one; on the ENCODE-rE2G CRISPR interference benchmark for enhancer-gene links, AlphaGenome 2 reached an AUPRC of 0.78 against 0.73 for AlphaGenome 1 and 0.61 for a Borzoi ensemble.

On single-cell tasks the improvement is clearer. The paper reports that AlphaGenome 2 explained 34% of the variance in expression between cell types within a tissue, against 23% for the fine-tuned first version, and the announcement illustrates the gain with the maturation of blood stem cells into their descendant cell types.

··········

ALPHAGENOME 2 EXPRESSION CONDITIONING FOR UNSEEN CELL STATES.

The feature with the most direct consequence for laboratories is expression conditioning. A researcher can supply a single-cell RNA sequencing snapshot from as few as a few dozen cells; the cell encoder turns it into a state representation and AlphaGenome 2 predicts regulatory activity for that state without any retraining.

Google DeepMind reports that this approach matched or beat models fine-tuned directly on the same datasets. Across 147 evaluation groups from the training split, expression conditioning outperformed dataset-specific fine-tuning with a reported P value of 3.1 × 10⁻¹², and it also won across 20 held-out test groups. Decoding from a single cell gave a median correlation of 0.61 with observed pseudobulk changes, rising to 0.87 when all cells in a group were averaged.

In a Parkinson's disease cohort the model had not seen, AlphaGenome 2 predicted the sign of chromatin accessibility QTLs with an AUC of 0.892, against 0.850 for CREsted. At the 87.3% sign accuracy that Scooby reaches, AlphaGenome 2 recovered 41% of variants against 17%.

The announcement also describes a COVID-19 mRNA vaccine example in which the model separated vaccine-activated T cells from resting ones and showed that a variant linked to severe COVID-19 disrupts gene activity more strongly in the activated state. For cross-modality imputation, the paper reports that AlphaGenome 2 beat the Avocado baseline on 166 of 181 tracks, and reached 1.7 to 2.9 times higher AUPRC than the better baseline on the rarest chromatin peaks.

··········

[[ADPLUS_300x250_1]]

ALPHAGENOME 2 FOR GWAS VARIANTS: THE PRECOMPUTED 510-CONDITION PANEL.

Alongside the model, Google DeepMind precomputed AlphaGenome 2 predictions for trait-associated variants. According to the paper, the panel scores 6,822,130 single-nucleotide variants across 510 human cell and tissue conditions, and a second resource maps enhancer-gene links in primary human cell types such as brain astrocytes, beyond the stable cell lines on which most earlier benchmarks were built.

The intended use is prioritization: narrowing a genome-wide association signal down to the likely causal variant, its target gene and the cell type where it acts. In the paper's analysis, 17 of 28 well-powered traits showed at least one significantly enriched condition, for example prostate luminal epithelium for prostate cancer and microglia for Alzheimer's disease. Predicted effect directions agreed with rare loss-of-function burden results in 75.4% of gene pairs.

The announcement highlights an inflammatory bowel disease case. A variant in a distant regulatory element overactivates the ETS2 gene in gut macrophages, a mechanism described in a 2024 Nature study. AlphaGenome 2 reproduced that result computationally, identified the causal variant (rs2836882 in the paper) and showed that its effect is selective to those cells.

··········

WHO CAN USE ALPHAGENOME 2: API ACCESS, COMMERCIAL PREVIEW AND RESTRICTIONS.

Access follows the model of the first version, with one addition for companies. The table summarizes what Google DeepMind has published.

........

User group

Access to AlphaGenome 2

Status

Academic and non-commercial researchers

AlphaGenome API plus Colab tutorials

Available since November 11, 2026

Commercial research teams

AlphaGenome API

Private preview

Google Antigravity users

AlphaGenome 2 Skills

Announced as coming soon

Community projects

Extra quota and credits for selected use cases

Submissions due November 30

Clinical and diagnostic use

Not permitted

Not validated or approved

........

Researchers who already use the AlphaGenome API reach the new model through the same service, documented at alphagenomedocs.com. Google DeepMind has not published prices or quota figures for AlphaGenome 2, and the announcement does not mention model weights, an open-source release or Google Cloud availability. For the first version, the company made the base model available for academic use on GitHub and through the API, and for commercial use through Google Cloud Model Garden, as it stated when it launched AlphaGenome Atlas in September 2026; no equivalent statement exists yet for AlphaGenome 2.

Two outside organizations are named in the launch. Peter Donnelly, chief executive of Genomics PLC, said the company plans to incorporate AlphaGenome 2 into its Mystra platform, and Sarah Teichmann, co-founder of the Human Cell Atlas, commented on the model's ability to adapt to new single-cell data without retraining. Google DeepMind also cites collaboration with researchers at the Wellcome Sanger Institute.

The planned Antigravity skills connect the model to Google's agentic development platform, where DeepMind has already published science workflows. Readers following Google DeepMind's research releases can compare the access model with that of Genie 4, released as a research preview, and with the lab's recent work on AI agents and legacy 1024-bit RSA.

··········


ALPHAGENOME 2 LIMITATIONS STATED BY GOOGLE DEEPMIND.

The paper is explicit about where the model falls short. At high precision, AlphaGenome 2 still recovers only a minority of fine-mapped expression QTLs and causal GWAS variants, so most true variants are missed at the confidence levels that would be used for follow-up experiments.

The authors write that the model can underestimate effects for variants outside accessible chromatin, at very distal regulatory elements, or acting after transcription. Transient, stimulated and disease states are underrepresented in both the training data and the GWAS panel, and the model struggles to separate fine-grained states such as subpopulations of neurons.

Because condition embeddings are derived from steady-state observational data, the authors say it remains unclear whether AlphaGenome 2 can predict responses to gene knockouts, overexpression or dynamic transitions. Running one prediction per condition also makes it more computationally expensive than fixed-track multitask models, a cost that grows with the number of cell types a study needs to examine.

··········

ALPHAGENOME 2: CONFIRMED FACTS AND OPEN QUESTIONS.

Google DeepMind has confirmed the model's conditional design, its one-million-base-pair input, the addition of CpG methylation, a training corpus of more than 100 million single-cell profiles across 68 tissues, and immediate availability through the AlphaGenome API for non-commercial research, with commercial access in private preview. The benchmark gains are largest on single-cell variant tasks, where recall at 90% precision roughly doubled on TenK10K, and smaller on bulk tissue tasks, where AlphaGenome 1 was already strong.

Several points remain undisclosed: the parameter count, pricing and quotas for API users, whether weights will follow as they did for the first version, a date for the Antigravity skills, and when commercial access will open beyond the private preview. All published results come from Google DeepMind's own evaluation, and the paper's limitations section sets clear bounds on its use for knockout prediction and rare disease states.

For research groups working on regulatory genetics, the practical gain is the ability to query variant effects in their own cell states from a few dozen cells of single-cell data, together with a precomputed panel of about 6.8 million variants across 510 conditions. The model remains a research tool, and Google DeepMind's restriction on clinical and diagnostic use applies to AlphaGenome 2 exactly as it did to the first version.
