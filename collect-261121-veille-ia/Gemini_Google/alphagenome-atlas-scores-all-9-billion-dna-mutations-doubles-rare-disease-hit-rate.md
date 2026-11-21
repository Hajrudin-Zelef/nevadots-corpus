---
id: collect-261121-veille-ia/gemini-google/alphagenome-atlas-scores-all-9-billion-dna-mutations-doubles-rare-disease-hit-rate
title: "AlphaGenome Atlas Scores All 9 Billion DNA Mutations, Doubles Rare-Disease Hit Rate"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-08"]
keywords: ["alphagenome", "atlas", "model", "billion", "deepmind", "human", "genome", "more"]
source: https://www.techtimes.com/articles/327056/20260909/alphagenome-atlas-scores-all-9-billion-dna-mutations-doubles-rare-disease-hit-rate.htm
source_anchor: ""
source_lines: [1, 89]
section: "Gemini_Google"
---
Google DeepMind released AlphaGenome Atlas on September 8, 2026 — a freely searchable, 1-petabyte database that pre-computes predicted molecular effects for all 9 billion possible single-letter changes in the human genome — and the attached variant-scoring system more than doubles researchers' ability to identify rare-disease-causing mutations compared to the field's previous standard tool. For the first time, a researcher anywhere in the world can look up any point mutation in the human genome and retrieve its predicted impact on gene expression, RNA splicing, and chromatin structure by opening a browser — no coding required, and no GPU cluster needed.

The Atlas is the latest step in DeepMind's decade-long effort to computationally decode life's instruction manual. AlphaFold, released in 2020, cracked the decades-old problem of predicting how proteins fold into their three-dimensional shapes, earning Demis Hassabis and John Jumper a share of the 2024 Nobel Prize in Chemistry. AlphaMissense, released in 2023, extended that lens to classify 71 million missense variants as likely benign or pathogenic. Both tools covered only the 2 percent of the genome that codes for proteins. AlphaGenome Atlas covers the other 98 percent — the regulatory dark matter where most trait-associated genetic variants actually live.

### What AlphaGenome Atlas Contains

The Atlas is built on top of AlphaGenome, an AI model DeepMind released in June 2025 and detailed in a *Nature* paper in January 2026. The model takes up to one million base pairs of DNA as input and produces predictions across 11 molecular modalities — including gene expression levels, RNA splicing patterns, chromatin accessibility, transcription factor binding, and three-dimensional genome contacts — across hundreds of human and mouse cell types and tissues.

To build the Atlas, DeepMind ran AlphaGenome across every possible single-letter substitution at every one of the roughly 3 billion positions in the human reference genome — a strategy called in silico saturation mutagenesis — and stored the results. The resulting dataset holds more than 100 million short insertions and deletions observed in human genomes as well, bringing the total to approximately 9 billion single-nucleotide variants, each with what amounts to thousands of molecular predictions attached.

The Atlas is more than 30 times larger than the AlphaFold Database in 2022, which itself was considered a landmark when it expanded to cover more than 200 million protein structure predictions. That scale reflects an engineering challenge that, until recently, seemed insurmountable. "When we started thinking about this project, it seemed impossible to do that computationally," said Žiga Avsec, the genomics lead at DeepMind. Early estimates suggested the team needed to improve their calculation speed by a factor of 80. They achieved it through a combination of model distillation, GPU kernel optimization, and elimination of redundant computations.

### How the Architecture Makes This Possible

AlphaGenome's core is a U-Net-style neural network: an encoder that compresses the raw one-million-base-pair DNA sequence, transformer blocks that capture long-range dependencies between distant regulatory elements, and a decoder that reconstructs base-pair-resolution predictions. The 1-megabase input is broken into 131-kilobase chunks processed in parallel using sequence parallelism across tensor processing units, as described in the AlphaGenome model architecture paper.

Pre-training required 256 Google TPUv3 chips working in parallel on data from four major genomics consortia: ENCODE, GTEx, 4D Nucleome, and FANTOM5. The model was then distilled — a process in which a smaller, faster "student" model learns to replicate the outputs of a slower, more accurate "teacher" ensemble. The distilled student model completes all-modality, all-cell-type predictions for a single variant in less than one second on an NVIDIA H100 GPU, making it practical to run across the full 9-billion-variant space.

The practical consequence of that speed is the Atlas itself: because inference is now fast enough to pre-compute everything, the bottleneck in genomics has fundamentally shifted. Before Atlas, the limiting factor in rare disease diagnosis was computational — which variants to test, how to run the model, what infrastructure to use. After Atlas, the limiting factor is experimental: researchers now have predictions for every possible mutation, but labs can only validate a small number of candidates in actual biological experiments. The database makes genomic dark matter searchable, but interpreting and confirming what it finds still requires human scientists and laboratory work.

### AVI Score: One Number for the Full Genome

Alongside the molecular predictions, DeepMind released the AlphaGenome Variant Impact score (AVI), a single composite number per variant that integrates AlphaGenome's regulatory predictions with pathogenicity estimates from AlphaMissense. The combined score covers both the coding and non-coding genome — unlike AlphaMissense, which only scored protein-altering variants in the coding 2 percent.

In head-to-head testing on solved rare-disease cases from the GREGoR Consortium, the AVI score placed the known causal variant in the top 50 candidates 29.5 percent of the time. CADD, the most widely used genome-wide variant scoring system, achieved 12.5 percent — less than half the AVI rate. That 2.4-times improvement on the benchmark that matters most to clinicians — finding the actual needle in the genomic haystack — is the clearest measure of what Atlas adds to the existing toolkit.

Each AVI score also comes with feature attributions that break down what is driving it: how much of the predicted disruption comes from RNA splicing, gene expression, chromatin accessibility, evolutionary conservation, or protein impact from AlphaMissense. A researcher can see not just that a variant scores high, but which molecular process it is predicted to disrupt.

The database also includes a compendium of more than 2,500 recurrent DNA motifs — the recurring "words" of the genome that include transcription factor binding sites — and their locations across the full genome.

### Rare Disease in Practice: DNM1 and the Epilepsy Case

The most specific clinical validation in the Atlas release comes from collaborators at the Broad Institute working with the GREGoR Consortium rare disease program on unsolved epilepsy cases. Researchers Laura Covill and Anne O'Donnell-Luria used AVI scoring to reprioritize variants that conventional filters had overlooked, surfacing a mutation in the *DNM1* gene — a gene strongly linked to epileptic encephalopathy.

What made the AVI-driven discovery useful was not just the ranking, but the mechanism it revealed: AlphaGenome's underlying predictions showed that the variant created an aberrant splice site, disrupting how the gene's RNA transcript is cut and assembled into a functional protein — specifically, causing an abnormal extension of the resulting protein. Laboratory confirmation followed, and experimental screens found additional nearby variants with similar effects. The case demonstrates the kind of hypothesis-to-experiment workflow the Atlas is designed to accelerate.

### Population Scale: 54,000 UK Biobank Genomes

Beyond individual rare disease cases, the Atlas showed its population-scale utility when Gareth Hawkes, a Medical Research Council fellow at the University of Exeter, applied it to UK Biobank whole-genome data from more than 54,000 participants.

By grouping rare variants according to their predicted molecular effects, Hawkes found 22 percent more non-coding genetic associations than standard statistical approaches would have detected in the background noise of population-scale sequencing. He pinpointed regulatory variants controlling circulating levels of two proteins: PLA2G7, linked to aging processes, and EGLN1, a cellular oxygen sensor. Filtering to the one percent of non-coding variants that Atlas rated as most impactful, he also identified 19 genomic regions associated with body mass index — a trait whose non-coding BMI genetic regions have been notoriously difficult to untangle.

A separate collaboration at the Stowers Institute for Medical Research used the Atlas's motif compendium to distinguish transcription factors at Stowers that only open up DNA from those that actively switch genes on — a distinction that was previously difficult to make without running separate experimental assays for each candidate.

### What the AVI Score Cannot Do

The Atlas's scope impresses, but several structural limits matter for anyone planning to use it. Carl de Boer, a genomicist at the University of British Columbia who helped develop a comparative framework for evaluating models like AlphaGenome, praised the Atlas as a useful resource while flagging the central interpretive risk. "It has a clear use, but it also is probably going to be easily misinterpreted," de Boer told IEEE Spectrum. "We're talking about a very complex system and there's a lot of moving parts."

The limitations are specific, not vague:

The model's field of view is bounded at 1 megabase (1 million base pairs). Some regulatory elements called enhancers can influence genes over much longer distances — sometimes exceeding the AlphaGenome 1-megabase input window — making their predicted effects unreliable.

The AVI score evaluates single variants in isolation. Most common complex diseases — type 2 diabetes, Alzheimer's disease, cardiovascular conditions — are linked to dozens or hundreds of interacting variants across the genome. A single-variant impact score captures none of those interactions.

Trans-acting effects — when a transcription factor's own expression level rises or falls due to variation elsewhere in the genome, causing downstream effects — sit outside the sequence window and cannot be predicted.

Compressing thousands of molecular tracks into a single AVI number aids ranking but can obscure the molecular mechanism in specific cases, particularly when evolutionary conservation dominates the score while the actual functional disruption is in a less-conserved pathway. The AVI feature attribution limitations are described in detail in the companion technical paper.

In at least one documented case — variants near the TAL1 gene in T-cell leukemia — the model's predictions did not match observed RNA and protein patterns well, partly because a short oncogenic isoform of the gene was poorly represented in the gene annotation data the model was trained on.

Professor Kristian Helin, CEO of the Institute of Cancer Research in London, called AlphaGenome a major advance in computational genomics while noting that cell type-specific regulation remains a particular challenge, according to expert reaction on AlphaGenome published by the Science Media Centre. The model's tissue labels are ontologies representing broad cell categories, not the precise living cell states in which regulatory elements operate differently.

DeepMind itself is explicit on the clinical question: AlphaGenome Atlas is "not approved for clinical use" and is "not intended to be a substitute for professional medical advice." Every high-scoring variant the Atlas surfaces remains a hypothesis that requires laboratory validation before it can inform a diagnosis.

### Does the Computation Bottleneck Explain Why This Matters?

The scale of the Atlas — and what made it difficult to build — illuminates the shift it represents. Before Atlas, around 9,000 researchers had accessed AlphaGenome's predictions through the API since its launch in June 2025, according to Nature AlphaGenome Atlas coverage citing Dhavi Hariharan, a DeepMind product manager. Using it required writing code, provisioning compute, and making decisions about which variants were worth scoring — a barrier that kept AlphaGenome in the hands of bioinformaticians and computational genomics labs.

The Atlas converts that per-query compute cost to zero. What a lab previously could not do without significant compute infrastructure — score every candidate variant in a 54,000-genome biobank study and filter to the top 1 percent — is now a database query. The same democratization effect that AlphaFold had on structural biology, where protein structures that once required years of crystallography became available in seconds, is what DeepMind is attempting for the regulatory genome.

Commercial Google Cloud access is expected in the coming months, alongside Isomorphic Labs, DeepMind's drug discovery affiliate, which will also require a commercial license. The non-commercial academic portal and API are live immediately at the AlphaGenome Atlas web portal.

## Frequently Asked Questions

### What is the AlphaGenome Atlas and how is it different from the AlphaGenome model?

AlphaGenome is an AI model that predicts the molecular effects of DNA variants when given a specific DNA sequence as input — but using it required coding skills and access to GPU computing resources. AlphaGenome Atlas is a pre-computed database that has already run the model across all 9 billion possible single-letter changes in the human genome and stored the results. Anyone can look up a variant through a free web portal without writing code or owning a GPU. Think of AlphaGenome as the laboratory and Atlas as the published encyclopedia it produced.

### Can AlphaGenome Atlas diagnose genetic diseases?

No. DeepMind is explicit that AlphaGenome Atlas is not approved or validated for any clinical use and is not a substitute for professional medical advice. The Atlas surfaces hypotheses — variants that are predicted to disrupt molecular processes — but every finding requires independent laboratory validation before it can inform a diagnosis. The AVI score tells a researcher which variants are most worth investigating first, not which ones are definitively causing disease. The tool is designed for academic research, not clinical practice.

### What does it mean that the computational bottleneck has shifted to experimental validation?

Before the Atlas, rare disease researchers faced two constraints: they did not know which of millions of candidate variants to score, and scoring them computationally was expensive and slow. The Atlas eliminates both constraints by pre-computing everything. What remains is the third constraint that has always existed: running an actual laboratory experiment to confirm whether a predicted effect is real. That step cannot be automated — it requires cells, reagents, equipment, and expertise. In practice, this means that the Atlas may generate more valid hypotheses than research labs have capacity to test, which represents a different kind of bottleneck than the field has faced before.

### Why does the Atlas only cover single-letter changes, and what does that leave out?

A single-nucleotide variant changes one DNA "letter" at a time — the most common type of genetic variation. The Atlas does include more than 100 million short insertions and deletions (indels) as well. What it does not cover is larger structural variations: copy number changes, chromosomal rearrangements, and complex polygenic interactions involving many variants acting together. Most common diseases involve multiple variants interacting across the genome, and no single-variant impact score — however good — can model that complexity. The Atlas is a powerful prioritization tool for the most common category of genetic variation, not a complete map of how genetic architecture produces complex traits.
