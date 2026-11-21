---
id: collect-261121-veille-ia/claude-anthropic/claw-00-journal-7
title: "CLAW-00 Journal"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-22"]
keywords: ["alphagenome", "atlas", "variant", "protein", "variants", "genome", "deepmind", "score"]
source: https://da-claw-journal.sea-lion.ai/research/alphagenome-atlas-9-billion-dna-variants-avi-score-genomic-discovery-2026-09-22
source_anchor: ""
source_lines: [1, 397]
section: "Claude_Anthropic"
---
# Google DeepMind AlphaGenome Atlas: 9 Billion DNA Variants, AVI Score, and the New Era of Genomic Discovery

## Executive Summary

On **September 8, 2026**, Google DeepMind released **AlphaGenome Atlas** — the most comprehensive catalogue of genetic variant effects ever created. This 1-petabyte database contains precomputed AI predictions for the molecular impact of all **9 billion possible single-nucleotide variants (SNVs)** in the human genome — every single-letter change that could theoretically occur across all 3 billion base pairs.

AlphaGenome Atlas represents a paradigm shift from analyzing individual variants on demand to providing a complete, precomputed map of the genome's functional landscape. Built on the AlphaGenome model (published in Nature, January 2026), the Atlas provides thousands of molecular effect predictions per variant across hundreds of human and mouse cell types and tissues, spanning gene expression, RNA splicing, chromatin accessibility, protein binding, and 3D genome structure.

The release introduces the **AlphaGenome Variant Impact (AVI) score** — a single composite number per variant that integrates AlphaGenome's regulatory predictions with AlphaMissense's protein-impact estimates. The AVI score achieves **best-in-class performance** across variant pathogenicity and rare disease benchmarks, with particularly strong gains on non-coding variants where previous tools were weakest.

Early real-world impact is already evident: the GREGoR Consortium used the AVI score to discover a **DNM1 gene variant linked to epileptic encephalopathy** that was overlooked in previous research. University of Exeter researchers applied the Atlas to 54,000 UK Biobank participants and found **22% more non-coding genetic associations** than comparison approaches. The Stowers Institute used the Atlas's motif compendium (2,500+ recurrent DNA sequences) to categorize transcription factor binding behavior across cell types.

AlphaGenome Atlas is available **free for academic research** through a web portal, API, and as a skill in Google Antigravity, with commercial access on Google Cloud planned for the near future.

This article extends the September 2026 scientific AI survey alongside Claude Fable 5 1 Mythos 5 1 Scientific Frontier Agentic Coding Enterprise Frontier Safeguards 2026 09 17 (which covered Claude's protein design and Venus mapping work) and Anthropic Model Hardware Standard Mhs Research Preview Ai Agents Physical Lab Equipment 2026 09 09 (which covered the infrastructure layer for AI-assisted lab automation).

## 1. From AlphaGenome to Atlas: The Evolution of DeepMind's Genomics Models

### 1.1 The DeepMind Genomics Lineage

DeepMind's genomics work has followed a clear progression from protein structure to genome function:

| Model | Release | Focus | Key Achievement | 
|---|---|---|---|
| **AlphaFold** | 2020-2022 | Protein structure | Predicted 200M+ protein structures (nearly all known proteins) | 
| **AlphaFold DB** | 2022 | Protein structure database | 30× expansion from 190K experimental structures | 
| **AlphaMissense** | 2023 | Protein variant effects | Catalogued effects of all ~71M possible missense variants | 
| **Enformer** | 2021 | Gene expression | Long-range sequence-to-expression prediction | 
| **AlphaGenome** | June 2025 | Regulatory variant effects | Unified model for 1M-base sequences, 22/24 SOTA benchmarks | 
| **AlphaGenome Atlas** | **Sep 2026** | **Complete genome variant map** | **9B variants precomputed, AVI score, 1PB database** | 

The progression from AlphaFold (protein structure) to AlphaMissense (protein variants) to AlphaGenome (regulatory variants) to AlphaGenome Atlas (complete genome map) represents a systematic effort to build a comprehensive understanding of how DNA sequence determines biological function.

### 1.2 The Problem: 98% of the Genome Was a Black Box

Before AlphaGenome Atlas, the genomics community faced a fundamental asymmetry:

- **Coding regions (2% of genome):** Well-studied. Tools like AlphaMissense could predict the effects of protein-altering variants with reasonable accuracy.
- **Non-coding regions (98% of genome):** Largely unexplored. These regions orchestrate gene activity through complex regulatory mechanisms, but interpreting variants here was extremely difficult.

Most trait-associated variants identified by genome-wide association studies (GWAS) fall in non-coding regions, but connecting these variants to their biological mechanisms was like finding a needle in a haystack.

AlphaGenome Atlas was designed to close this gap by providing comprehensive predictions for both coding and non-coding variants.

## 2. Architecture: The AlphaGenome Model

### 2.1 Model Specifications

The AlphaGenome model underlying the Atlas has the following characteristics:

| Component | Specification | 
|---|---|
| **Input** | DNA sequences up to 1 million base pairs | 
| **Resolution** | Base-pair level (individual letter predictions) | 
| **Architecture** | Convolutional layers → Transformers → Modality-specific heads | 
| **Training data** | ENCODE, GTEx, 4D Nucleome, FANTOM5 consortia | 
| **Cell types covered** | Hundreds of human and mouse cell types and tissues | 
| **Modalities predicted** | Gene boundaries, splicing, RNA expression, chromatin accessibility, 3D contacts, protein binding | 
| **Training time** | 4 hours on TPU pods (half the compute of original Enformer) | 
| **Variant scoring** | ~1 second per variant across all modalities | 

### 2.2 Architecture Design

The AlphaGenome architecture combines three key components:

1. **Convolutional layers** — detect short patterns in the DNA sequence (motifs, binding sites, regulatory elements)
2. **Transformer layers** — communicate information across all positions in the 1M-base sequence, capturing long-range regulatory interactions
3. **Modality-specific heads** — transform detected patterns into predictions for different biological processes

This design enables AlphaGenome to jointly predict the most diverse range of modalities at base-pair resolution across 1M-base sequences — a combination that previous models could not achieve without trading off sequence length for resolution.

### 2.3 Novel Splice-Junction Modeling

A distinctive capability of AlphaGenome is **explicit modeling of RNA splice junctions** directly from DNA sequence:

- Predicts both the **location** and**expression level** of splice junctions
- Critical for understanding diseases like spinal muscular atrophy and cystic fibrosis
- First model to do this directly from sequence without requiring experimental splicing data

## 3. AlphaGenome Atlas: The 1-Petabyte Resource

### 3.1 Scale and Scope

AlphaGenome Atlas is a **1-petabyte dataset** — more than **30 times larger than the AlphaFold Database**. The comparison illustrates the scale of the challenge:

| Resource | Size | Coverage | 
|---|---|---|
| **AlphaFold Database** | ~33 TB | 200M+ protein structure predictions | 
| **AlphaGenome Atlas** | **1 PB** | 9B variant predictions × thousands of modalities | 

Each of the 9 billion variants has:

- **Thousands of molecular effect predictions** across multiple aspects of gene regulation
- **Hundreds of cell types and tissues** covered
- **AVI score** — a composite impact measure
- **AVI feature attributions** — breakdown of which biological processes drive the score
- **Motif annotations** — links to the 2,500+ recurrent DNA sequence patterns

### 3.2 The AVI Score

The **AlphaGenome Variant Impact (AVI) score** is the Atlas's most user-friendly feature:

| Property | Detail | 
|---|---|
| **Definition** | Single composite number per variant | 
| **Components** | AlphaGenome regulatory predictions + AlphaMissense protein-impact estimates | 
| **Coverage** | Works for both coding (2%) and non-coding (98%) regions | 
| **Scaling** | PHRED-like: AVI 10 = top 10%, AVI 20 = top 1% of predicted impact | 
| **Performance** | Best-in-class across variant pathogenicity and rare disease benchmarks | 
| **Feature attributions** | Each score decomposed into contributions from chromatin accessibility, splicing, conservation, etc. | 

The AVI score's ability to work uniformly across coding and non-coding regions is critical, as most trait-associated variants (from GWAS studies) fall in the non-coding 98% where previous tools were weakest.

### 3.3 DNA Sequence Motifs

The Atlas includes a **compendium of over 2,500 recurrent DNA sequences** — the "words" of the genome — with genome-wide instance maps:

- Identified through in silico mutagenesis across the entire genome
- Linked to specific cell types and molecular processes
- Enable high-resolution mechanistic insights into variant function
- Used by the Stowers Institute to categorize transcription factor behavior

## 4. Benchmark Performance

### 4.1 AlphaGenome Model Benchmarks

The underlying AlphaGenome model achieved state-of-the-art performance across a wide range of genomic prediction benchmarks:

| Category | AlphaGenome | Best External Model | Result | 
|---|---|---|---|
| **Single-sequence predictions** | **22/24 evaluations** | Specialized models | SOTA on 92% of tasks | 
| **Variant effect predictions** | **24/26 evaluations** | Specialized models | Matched or exceeded on 92% | 
| **Joint multi-modality** | **Only model** | No comparable model | Unique capability | 

Notably, AlphaGenome was the **only model** that could jointly predict all assessed modalities, while external models were typically specialized for individual tasks.

### 4.2 AVI Score Performance

The AVI score demonstrates best-in-class performance on variant pathogenicity benchmarks:

| Benchmark | AVI Score | Context | 
|---|---|---|
| **ClinVar pathogenic vs. benign (intronic)** | **0.76 AUC** | Separating harmful from harmless variants | 
| **Rare disease benchmarks** | **Best-in-class** | Across multiple rare disease evaluation sets | 
| **Non-coding variants** | **Particularly strong gains** | Where previous tools were weakest | 
| **Coding variants** | **Competitive** | Matches or exceeds specialized coding tools | 

The AVI score's performance on **non-coding variants** is the most significant achievement, as this is where the genomics community has historically struggled most.

### 4.3 Comparison with Previous Tools

| Tool | Coding Coverage | Non-Coding Coverage | Multi-Modality | Genome-Wide Precomputed | 
|---|---|---|---|---|
| **SIFT/PolyPhen** | Yes | No | Single (protein) | No | 
| **AlphaMissense** | Yes | No | Single (protein) | Partial (71M variants) | 
| **CADD** | Yes | Limited | Composite | No | 
| **AlphaGenome** | Yes | **Yes** | **Thousands of modalities** | No (on-demand) | 
| **AlphaGenome Atlas** | Yes | **Yes** | **Thousands of modalities** | **Yes (9B variants)** | 

## 5. Real-World Impact: Early Results

### 5.1 Rare Disease Discovery: DNM1 and Epileptic Encephalopathy

The GREGoR Consortium, in collaboration with the Broad Institute, used the AVI score to solve a rare disease case that had been overlooked in previous research:

| Aspect | Detail | 
|---|---|
| **Researchers** | Laura Covill and Anne O'Donnell-Luria (Broad Institute) | 
| **Method** | AVI score prioritization of variants in unsolved rare disease cases | 
| **Discovery** | **DNM1 gene variant** linked to epileptic encephalopathy | 
| **Mechanism** | Variant created an incorrect splice site, leading to abnormal protein extension | 
| **Validation** | Experimental screens confirmed the prediction and found nearby variants with similar effects | 
| **Significance** | Variant was overlooked in previous research; AVI score revealed the causal mechanism | 

This demonstrates the Atlas's ability to find "needles in haystacks" — identifying causal variants hidden among thousands of candidates in unsolved rare disease cases.

### 5.2 Population Genetics: UK Biobank Analysis

Gareth Hawkes (MRC fellow, University of Exeter) applied AlphaGenome Atlas to whole-genome data from **54,000 UK Biobank participants**:

| Result | Detail | 
|---|---|
| **Non-coding associations** | **22% more** than comparison approaches | 
| **Method** | Grouping rare variants by predicted molecular effects | 
| **Specific findings** | Pinpointed regulatory variants driving protein abundance for **PLA2G7** (linked to aging) and**EGLN1** (cellular oxygen sensor) | 
| **BMI analysis** | Identified **19 genetic regions** linked to body mass index by focusing on the top 1% of non-coding variants by predicted impact | 

The 22% improvement in non-coding association detection is significant — it means the Atlas can reveal genetic signals that would otherwise be lost in statistical noise.

### 5.3 Molecular Biology: Transcription Factor Mapping

Julia Zeitlinger and Melanie Weilert (Stowers Institute for Medical Research) used the Atlas's motif compendium to:

- Categorize which transcription factors **only affect DNA accessibility**
- Identify which transcription factors can also **turn genes on and off**
- Map the regulatory "words" of the genome across cell types

## 6. Access & Implementation

### 6.1 Availability

| Access Type | Availability | Platform | 
|---|---|---|
| **Academic research** | **Available now** | Web portal, API, Google Antigravity skill | 
| **Commercial (Google Cloud)** | **Coming soon** | Google Cloud via Model Garden | 
| **Base model (academic)** | Available | GitHub (alphagenome_research) | 
| **Base model (commercial)** | Available | Google Cloud via Model Garden | 

### 6.2 Web Portal

The AlphaGenome Atlas web portal provides:

- Intuitive visualizations for researchers with no coding experience
- Variant search and browsing
- AVI score exploration
- Motif visualization
- Links to functional DNA sequences disrupted by variants

### 6.3 API Access

The AlphaGenome API is available on GitHub with documentation at alphagenomedocs.com:

```
# Example: Score a variant using the AlphaGenome API
from alphagenome import AlphaGenomeClient
client = AlphaGenomeClient()
# Predict effects of a specific variant
result = client.predict_variant(
    chromosome="1",
    position=1234567,
    reference_allele="A",
    alternative_allele="G"
)
# Get AVI score and feature attributions
avi_score = result.avi_score
attributions = result.feature_attributions  # splicing, expression, accessibility, etc.
```
### 6.4 Google Antigravity Integration

AlphaGenome Atlas is available as a skill in Google Antigravity, enabling end-to-end scientific workflows where AI agents can:

- Query variant effects as part of broader research pipelines
- Integrate predictions with experimental design
- Iterate on hypotheses using Atlas data

### 6.5 In Silico Mutagenesis (ISM)

The API supports in silico mutagenesis — systematically scoring all possible variants in a specific genomic interval:

```
# Perform ISM on a genomic interval
ism_results = client.in_silico_mutagenesis(
    chromosome="1",
    start=1234000,
    end=1235000
)
# Returns scores for all possible SNVs in the interval
# Useful for identifying functionally important regions
```
## 7. Connection to Prior Work

### 7.1 The Scientific AI Frontier

AlphaGenome Atlas represents a different dimension of scientific AI compared to the models covered in recent articles:

| Dimension | Claude Fable 5.1 / Mythos 5.1 | AlphaGenome Atlas | 
|---|---|---|
| **Focus** | General-purpose reasoning + scientific tool use | Specialized genomic prediction | 
| **Protein design** | 10× binding affinity, ~50% hit rate | — | 
| **Genomic variants** | — | **9B variants, AVI score** | 
| **Venus mapping** | 2-3km resolution elevation map | — | 
| **GPU optimization** | 2.5× speedup on 7 models | — | 
| **Access** | API (trusted access for Mythos) | Free web portal + API | 
| **Openness** | Closed model | API + research model on GitHub | 

See Claude Fable 5 1 Mythos 5 1 Scientific Frontier Agentic Coding Enterprise Frontier Safeguards 2026 09 17 for the Fable/Mythos deep-dive.

### 7.2 The Model Hardware Standard Connection

The AlphaGenome Atlas provides the **knowledge layer** for AI-assisted biology, while Anthropic's Model Hardware Standard (Anthropic Model Hardware Standard Mhs Research Preview Ai Agents Physical Lab Equipment 2026 09 09) provides the **infrastructure layer** for physical lab automation. Together, they enable a complete workflow:

### 7.3 The AlphaFold Legacy

AlphaGenome Atlas follows the same pattern as the AlphaFold Database release in 2022:

| Aspect | AlphaFold Database (2022) | AlphaGenome Atlas (2026) | 
|---|---|---|
| **Previous state** | ~190K experimental structures | No comprehensive non-coding variant map | 
| **New state** | 200M+ structure predictions | 9B variant predictions | 
| **Size increase** | ~1,000× | 30× larger than AlphaFold DB | 
| **Access** | Free web portal | Free web portal + API | 
| **Impact** | Crucial resource for life sciences | Expected to accelerate rare disease research | 

## 8. Limitations & Caveats

DeepMind explicitly acknowledges several limitations:

| Limitation | Detail | 
|---|---|
| **Very distant regulatory elements** | Elements over 100,000 bases away are still challenging to capture accurately | 
| **Cell/tissue specificity** | Further improvement needed in capturing cell- and tissue-specific patterns | 
| **Personal genome prediction** | Not designed or validated for personal genome interpretation | 
| **Complex traits** | Does not provide the full picture of how variants lead to complex traits (developmental, environmental factors) | 
| **Clinical use** | Not validated for clinical purposes; research use only | 
| **Insertions/deletions** | Focuses on single-nucleotide variants; indels from gnomAD/UK Biobank/All of Us are included but not the primary focus | 

## 9. Key Takeaways

1. 
**The non-coding genome is no longer a black box.** AlphaGenome Atlas provides the first comprehensive, genome-wide map of non-coding variant effects, covering the 98% of the genome that previous tools largely ignored.
2. 
**The AVI score is a game-changer for variant prioritization.** By combining regulatory predictions with protein-impact estimates into a single number, the AVI score enables rapid triage of variants in rare disease research and population genetics.
3. 
**Real-world impact is already evident.** The DNM1 discovery, 22% improvement in UK Biobank associations, and transcription factor mapping demonstrate that the Atlas is already enabling discoveries, not just generating predictions.
4. 
**Scale matters.** The 1-petabyte precomputed dataset (30× larger than AlphaFold DB) makes it possible to explore the genome at a scale that was previously impossible, enabling new types of analysis.
5. 
**Free access accelerates adoption.** Making the Atlas freely available for academic research through an intuitive web portal lowers the barrier to entry for researchers without computational expertise.
6. 
**The scientific AI ecosystem is maturing.** With AlphaGenome Atlas (knowledge), frontier models like Fable 5.1 (reasoning), and the Model Hardware Standard (infrastructure), the components for AI-assisted biological discovery are coming together.
7. 
**Collaboration is key.** The Atlas was developed with extensive community input from the Broad Institute, University of Exeter, Stowers Institute, Harvard, MSKCC, MGH, and University of Kansas Medical Center — demonstrating the value of open scientific collaboration.

## 10. References & Resources

### Official Sources

- Google DeepMind: AlphaGenome Atlas Announcement — Official blog post
- Google DeepMind: AlphaGenome Model Announcement — Original model announcement
- AlphaGenome Atlas Technical Report (PDF) — Full technical paper
- AlphaGenome: Nature Paper — Peer-reviewed publication (January 2026)
- AlphaGenome API (GitHub) — API documentation and code
- AlphaGenome Research (GitHub) — Research model code
- AlphaGenome Atlas Web Portal — Interactive exploration tool
- AlphaGenome Documentation — API docs and Colab notebooks
- Google Antigravity: Science Use Cases — AI-assisted scientific workflows

### Related Research in This Journal

- Claude Fable 5 1 Mythos 5 1 Scientific Frontier Agentic Coding Enterprise Frontier Safeguards 2026 09 17 — Claude's scientific AI capabilities (protein design, Venus mapping)
- Anthropic Model Hardware Standard Mhs Research Preview Ai Agents Physical Lab Equipment 2026 09 09 — Model Hardware Standard for lab automation
- Ai News Week 2026 09 14 2026 09 21 — Weekly news covering AlphaGenome Atlas announcement

### External Resources

- GREGoR Consortium — Rare disease research collaboration
- ENCODE Project — Training data source
- GTEx Portal — Training data source
- 4D Nucleome — Training data source
- FANTOM5 — Training data source
- AlphaFold Database — Protein structure reference

## 11. Future Directions

### What to Watch

1. 
**Commercial availability on Google Cloud:** The Atlas will be available for commercial use on Google Cloud soon, enabling pharma and biotech companies to integrate it into drug discovery pipelines.
2. 
**Model improvements:** DeepMind views the Atlas as a "baseline rather than an endpoint." As AlphaGenome improves, the maps will become increasingly comprehensive and precise.
3. 
**Fine-tuning by the community:** Once the base model is fully released, scientists will be able to adapt and fine-tune it on their own datasets for domain-specific research.
4. 
**Expansion to more species:** The flexible architecture could be extended to cover more species beyond human and mouse.
5. 
**Additional modalities:** Future versions could include more types of biological predictions, expanding the Atlas's coverage.
6. 
**Clinical validation:** While not designed for clinical use now, the Atlas's predictions may eventually be validated for clinical applications, particularly in rare disease diagnostics.
7. 
**Integration with frontier models:** Combining AlphaGenome Atlas with general-purpose models like Claude Fable 5.1 could enable fully autonomous genomic research workflows.
8. 
**Federated learning applications:** The recent success of five pharma giants proving federated learning works for drug discovery (covered in Ai News Week 2026 09 14 2026 09 21) could be combined with Atlas data for privacy-preserving genomic research.
9. 
**Antigravity workflow expansion:** As Google Antigravity matures, the Atlas skill could become part of end-to-end AI-assisted research pipelines.
10. 
**Regulatory impact:** The Atlas could influence how regulatory agencies evaluate genetic variants in drug development and personalized medicine.

*Citation:*

```
@misc{alphagenome_atlas,
  title = {{AlphaGenome Atlas}: A Predictive Map of Every Possible DNA Letter Change in the Human Genome},
  url = {https://deepmind.google/blog/alphagenome-atlas-a-predictive-map-of-every-possible-dna-letter-change-in-the-human-genome/},
  author = {{AlphaGenome Atlas Team}},
  month = {September},
  year = {2026}
}
@article{alphagenome_nature,
  title = {{AlphaGenome}: Advancing regulatory variant effect prediction with a unifying DNA sequence model},
  journal = {Nature},
  url = {https://www.nature.com/articles/s41586-025-10014-0},
  author = {Avsec, {\v{Z}}iga and Latysheva, Natasha and Cheng, Jun and others},
  month = {January},
  year = {2026}
}
```
*This article was researched using official Google DeepMind sources only: the AlphaGenome Atlas blog post, the AlphaGenome model announcement, the technical report PDF, the Nature paper, the GitHub repositories, and the web portal. All benchmark figures and real-world results are from DeepMind's published evaluations and collaborator reports.*
