---
id: collect-261121-veille-ia/gemini-google/alphagenome-atlas-maps-9-billion-human-dna-variants
title: "AlphaGenome Atlas Maps 9 Billion Human DNA Variants"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "atlas", "variants", "human", "billion", "changes", "effects", "possible"]
source: https://www.medindia.net/news/alphagenome-atlas-maps-9-billion-human-dna-variants-225139-1.htm
source_anchor: ""
source_lines: [1, 156]
section: "Gemini_Google"
---
## Artificial intelligence may help scientists interpret the 98% of human DNA that regulates gene activity without directly coding for proteins. AlphaGenome Atlas predicts the effects of 9 billion possible genetic changes, helping researchers study rare diseases and complex traits.


**How can scientists investigate billions of possible DNA changes without testing every one individually in a laboratory?**

**AlphaGenome Atlas, a large artificial intelligence-powered database that predicts the molecular effects of approximately 9 billion possible single-letter changes in the human genome**.

The resource covers both the small portion of DNA that provides instructions for making proteins and the much larger non-coding portion that helps control when, where and how genes work.

By making these predictions searchable through an online portal, AlphaGenome Atlas could help researchers identify genetic changes that deserve closer investigation in rare diseases, common traits and molecular biology (1

<sup>✔</sup>).

However, its predictions are intended to guide research and should not be treated as confirmed diagnoses or replacements for laboratory testing and clinical interpretation.

## Atlas Predicts 9 Billion Possible DNA Changes

The human genome contains about
**3 billion DNA base pairs**. At almost every position, one DNA letter can potentially be replaced by one of three alternatives, producing roughly 9 billion possible single-nucleotide variants.

Testing all these possible changes experimentally would require enormous amounts of time, money and biological material. AlphaGenome Atlas addresses this challenge by using Google DeepMind’s AlphaGenome model to calculate their predicted molecular effects in advance.

The resulting database is approximately

**1 petabyte in size—more than 30 times larger than the AlphaFold Database**, according to Google DeepMind (2

<sup>✔</sup>).

Researchers can examine how a variant may affect processes such as gene expression, RNA splicing and chromatin accessibility across hundreds of human and mouse cell types and tissues.

## Most Human DNA Does Not Directly Code for Proteins

Only about
**2% of the human genome directly codes for proteins**. Scientists understand many variants within these protein-coding regions because their effects can often be connected to changes in protein structure or function.

The remaining

**98% is largely non-coding DNA**. Although it does not directly provide protein-building instructions, much of it helps regulate gene activity.

Changes in non-coding regions can alter when a gene is activated, how strongly it is expressed or how RNA is processed. These effects can contribute to

AlphaGenome Atlas aims to make this less-studied part of the genome easier to investigate by providing predicted molecular consequences for variants across both coding and non-coding regions.

## AVI Score Helps Researchers Prioritize Variants

Each genetic variant can influence numerous biological processes, producing thousands of individual predictions. Reviewing all these measurements would be impractical when researchers are evaluating large numbers of variants.
Google DeepMind has therefore introduced the

**AlphaGenome Variant Impact score, or AVI score**. It combines predictions from AlphaGenome and AlphaMissense into a single value representing the predicted impact of a variant.

The system is designed to work across both protein-coding and non-coding DNA. Researchers can use the score to rank large groups of variants and identify those that may warrant laboratory testing or closer clinical investigation.

The Atlas also provides feature-attribution information explaining which predicted biological effects contributed to the score. These may include disruptions involving RNA splicing, gene expression, chromatin accessibility, evolutionary conservation or protein function.

This additional information is important because two variants with similar overall scores may affect biology through very different mechanisms (2

<sup>✔</sup>).

## Four Resources Support Genetic Variant Research

AlphaGenome Atlas connects several types of genomic information:
- **Molecular-effect predictions:** Predicted effects across several aspects of gene regulation and numerous cell types and tissues
- **AVI scores:** A single value designed to help scientists rank potentially important variants
- **Feature attributions:** Information showing which biological features contributed to each impact score
- **DNA sequence motifs:** More than 2,500 recurring DNA patterns and their locations across the genome

The sequence motifs may also help researchers locate regions where transcription factors—proteins that regulate whether genes are turned on or off—interact with DNA.

## Rare Disease Researchers Identified a DNM1 Variant

One potential application of AlphaGenome Atlas is investigating rare diseases that remain unexplained after conventional genetic analysis.
A person’s genome can contain thousands of unusual variants, but only a small number may be related to the condition under investigation. Identifying the relevant change can therefore resemble searching for a needle in a haystack.

Researchers associated with the Broad Institute and the Genomics Research to Elucidate the Genetics of Rare Diseases Consortium used the AVI score to prioritize variants in an unresolved rare-disease case.

The analysis highlighted a variant in the

**DNM1 gene**, which has been associated with epileptic encephalopathy. AlphaGenome predicted that the variant created an incorrect RNA splice site, potentially causing an abnormal extension of the resulting protein.

Experimental screening supported the prediction and identified nearby variants with similar effects. This case demonstrates how computational predictions may help researchers select variants for targeted laboratory validation (2

<sup>✔</sup>).

It does not mean that every highly ranked variant causes disease. Clinical evidence, family history, population data and experimental findings remain necessary when determining whether a genetic change is medically significant.

## UK Biobank Analysis Found More Genetic Associations

AlphaGenome Atlas has also been applied to whole-genome information from
**more than 54,000 UK Biobank participants**.

Rare non-coding variants are particularly difficult to study because large numbers of harmless changes can create statistical noise. Researchers attempted to reduce this problem by grouping variants according to their predicted molecular effects.

According to Google DeepMind, the approach uncovered

**22% more non-coding genetic associations**than would otherwise have been detected. It also helped identify regulatory variants connected with levels of circulating proteins, including PLA2G7 and EGLN1.

When the analysis focused on the

**top 1% of non-coding variants predicted to have the greatest impact**, researchers identified 19 genetic regions associated with body mass index.

These findings provide starting points for further investigation rather than proving that the identified variants directly determine a person’s weight or health (2

<sup>✔</sup>).

## Atlas Could Accelerate Several Areas of Research

Researchers may use AlphaGenome Atlas to:
- Prioritize candidate variants in unsolved rare diseases
- Investigate how non-coding DNA regulates gene activity
- Identify variants associated with proteins and complex human traits
- Study changes affecting RNA splicing or gene expression
- Select promising variants for laboratory validation
- Explore potential biological pathways and therapeutic targets

This could be especially useful during the early stages of research, when teams need to narrow thousands of possible variants to a manageable number for deeper study.

## Free Website Opens Access Beyond Coding Specialists

AlphaGenome Atlas is available to academic researchers through a free online portal designed for people who may not have programming experience (3
<sup>✔</sup>).

Researchers can explore genomic regions, inspect variant scores and review predicted molecular effects through the website. Programmatic access is also available through the AlphaGenome application programming interface, allowing computational scientists to incorporate the predictions into larger research workflows .

The underlying AlphaGenome research resources are also available through GitHub for academic use. Google DeepMind says commercial access is expected through Google Cloud.

Making the information accessible through both visual and computational tools could broaden its use among geneticists, molecular biologists, clinical researchers and data scientists.

## Predictions Still Require Experimental Confirmation

AlphaGenome Atlas is a research tool, not a clinical genetic test. Its scores estimate which variants may affect molecular biology, but a prediction alone cannot establish that a variant causes a disease.
Genetic effects may depend on the tissue involved, interactions with other genes, environmental exposures and factors not fully represented by an artificial intelligence model.

Researchers must therefore combine Atlas predictions with laboratory experiments, patient information, family studies and established standards for interpreting genetic variants.

The database may be most valuable as a prioritization system—helping scientists decide where to focus limited experimental resources—rather than as a source of final answers.

## AlphaGenome Atlas Expands the Map of Human Biology

AlphaGenome Atlas represents an effort to make the enormous number of possible human DNA changes more manageable for researchers.
Its scale is striking:

**9 billion predicted variants, a 1-petabyte dataset, more than 2,500 DNA motifs and molecular predictions spanning hundreds of cell types and tissues**.

Early applications suggest that the resource may help scientists investigate previously overlooked variants, particularly within the 98% of the genome that does not directly code for proteins.

**The Atlas will not eliminate the need for experiments or clinical expertise, but it could help researchers reach the most promising questions faster.**

As genomic artificial intelligence models improve, databases such as AlphaGenome Atlas may become increasingly useful for understanding genetic disorders, studying complex traits and identifying targets for future biomedical research.

**References:**

1. AlphaGenome Atlas: A High-Resolution Map of Human DNA – Google (https://blog.google/innovation-and-ai/models-and-research/google-deepmind/alphagenome-atlas/)
2. AlphaGenome Atlas: A Predictive Map of Every Possible DNA Letter Change in the Human Genome – Google DeepMind (https://deepmind.google/blog/alphagenome-atlas-a-predictive-map-of-every-possible-dna-letter-change-in-the-human-genome/)
3. AlphaGenome Atlas Website Portal – Google DeepMind (https://alphagenome.google/atlas)

Source-Medindia
