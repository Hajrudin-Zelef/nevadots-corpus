---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-review-dna-variant-effects-by-cell-type
title: "AlphaGenome 2 Review: DNA Variant Effects by Cell Type"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "cell", "model", "type", "variant", "deepmind", "single-cell", "effects"]
source: https://evermx.com/case/alphagenome-2
source_anchor: ""
source_lines: [1, 115]
section: "Gemini_Google"
---
# AlphaGenome 2 Review: DNA Variant Effects by Cell Type

AlphaGenome 2 predicts DNA variant effects per cell type and state; DeepMind says it finds nearly twice as many expression-altering variants.

## Introduction

On November 11, 2026, Google DeepMind announced AlphaGenome 2, a DNA sequence model that predicts how genetic variants change gene regulation in specific human cell types and cell states. The original AlphaGenome, and the AlphaGenome Atlas built on top of it, worked at the level of broad tissues and cell lines. AlphaGenome 2 zooms in. According to DeepMind, it "identifies nearly twice as many variants that change gene expression in cells" as the company's previous state-of-the-art model, and it is available today for non-commercial research through the AlphaGenome API, with a private preview for commercial research. The release matters because, as the accompanying paper puts it, most variants associated with human traits and diseases lie in non-coding DNA, where their effect depends on the cell type and state reading the sequence.

## Feature Overview

**A conditional model instead of parallel tracks.** The first AlphaGenome took a one-million-letter DNA sequence and predicted thousands of experimental tracks from many tissues at once. DeepMind says that spread the model's capacity thin and made closely related cell types hard to tell apart. AlphaGenome 2 is instead a conditional model: it takes the DNA sequence plus a target condition, defined as a tissue, cell line, cell type or cell state, and predicts one condition at a time. Per the paper, the condition is a learned embedding that modulates the network from its earliest layers through the encoder, transformer tower and decoder. It can be picked from a catalog of reference conditions or generated from a researcher's own single-cell RNA-seq data by a separate cell encoder.

**Training data and readouts.** The paper describes four training stages, including distilling eight teacher models into one student, and adds thousands of single-cell pseudobulk tracks plus cell-type-resolved DNA methylation. The cell encoder was trained on more than 100 million human single-cell expression profiles across 68 tissues. The model predicts the same core readouts as before, such as gene expression, histone modifications and chromatin accessibility, and adds DNA methylation, which DeepMind calls a widely requested readout. Because conditions are separated from output heads, supplying one measurement, such as single-cell gene activity, lets the model impute unmeasured properties like chromatin accessibility.

**Adapting without retraining.** A scientist can upload a single-cell RNA-seq snapshot from as few as a few dozen cells, and the model uses it as the condition for cell states it has not seen. DeepMind says this can match or outperform models trained or fine-tuned directly on those datasets.

**Benchmarks.** The headline gains come from the high-precision regime that matters for variant prioritization. The paper reports recall at 90% precision, meaning how many true causal variants the model finds while keeping nine in ten of its calls correct.

| Benchmark | Metric | AlphaGenome 2 | Baseline | 
|---|---|---|---|
| TenK10K causal sc-eQTLs (28 immune cell types) | Recall at 90% precision | 30% | 16% (AlphaGenome 1 fine-tuned on single-cell data) | 
| TenK10K sc-eQTL effect direction | Recall at 90% sign accuracy | 43% | 23% (same baseline) | 
| GTEx bulk eQTL causality (49 tissues) | Recall at 90% precision | 26% | 17% (AlphaGenome 1) | 
| TenK10K causal sc-eQTLs | Overall AUPRC | 0.76 | 0.72 (AlphaGenome 1 fine-tuned), 0.69 (Scooby) | 

The last row is the useful reality check. Across the whole precision-recall curve the gain is four hundredths of AUPRC, so the near-doubling applies specifically where the model is held to high precision. On splicing, chromatin accessibility, DNase and polyadenylation QTL benchmarks, the paper reports performance comparable to AlphaGenome 1.

**Precomputed resources.** DeepMind also released genome-wide enhancer-gene links across 640 primary single-cell reference conditions and precomputed scores for about 7 million variants, drawn from fine-mapped GWAS credible sets in the Open Targets Platform and common variants from gnomAD, across 510 conditions. The enhancer-gene work extends to primary cell types such as brain astrocytes, where CRISPR screens, which mostly use lab-grown cell lines, leave gaps.

## Usability Analysis

Access is through the AlphaGenome API, with Colab tutorials for expression conditioning. The public documentation lists tutorials on choosing a condition, conditioning on your own single-cell data and working with imputed tracks, a guide for migrating from AlphaGenome 1, and vignettes on the ETS2, TAL1 and BCL11A loci. DeepMind says AlphaGenome 2 Skills for its Antigravity platform are coming, and it is inviting use-case submissions by November 30, with additional quota and credits possible for selected projects.

Two worked examples show the intended workflow, and both are retrospective. At an inflammatory bowel disease locus, an earlier study had traced the risk variant rs2836882 to the ETS2 gene roughly 290 kb away in macrophages. Scoring the seven variants in the fine-mapped credible set, AlphaGenome 2 predicted a substantial effect for only that variant, linked it to ETS2 and confined it to monocytes and macrophages. In a COVID-19 mRNA vaccine dataset the model had not seen, it separated vaccine-activated T cells from resting ones and indicated that a severe-COVID-linked variant has a stronger effect in the activated state. These reproduce or nominate biology rather than establish new findings.

Cost is the practical constraint. Žiga Avsec told The Verge that AlphaGenome 2's predictions are "more expensive" than its predecessor's, and The Verge reports that those demands would have made the genome-wide Atlas impractical to produce with it. The paper likewise notes that it needs a separate prediction for each condition. AlphaGenome 1 stays relevant.

## Pros and Cons

**Strengths**

- Conditioning on a cell type or state targets the variants that act in one cell population and are averaged away in tissue-level models.
- Recall at 90% precision nearly doubles on the TenK10K single-cell eQTL benchmark.
- Expression conditioning lets researchers use their own single-cell data without retraining.
- Precomputed enhancer-gene links and variant scores spare most users large-scale computation.

**Limitations**

- Per-condition prediction costs more compute than AlphaGenome 1.
- The paper says that at high precision the model recovers only a minority of fine-mapped causal variants, and it can underestimate effects outside accessible chromatin.
- Transient, stimulated and disease states are underrepresented in training and in the precomputed GWAS panel.
- DeepMind states the models are for research use only and have not been validated or approved for clinical or diagnostic purposes.

## Outlook

DeepMind says it is working with groups including the Wellcome Sanger Institute to profile a wider range of cell types, developmental lineages and perturbed states. Avsec told The Verge that future versions could train on data from multiple species and that models like this could eventually feed into "virtual cells." The paper adds that condition embeddings learned from steady-state profiles reflect correlation rather than causation, and that it remains unclear whether the model can predict responses to gene knockouts or dynamic transitions. Partners quoted in the DeepMind post, including Sarah Teichmann of the University of Cambridge and Peter Donnelly of Genomics Limited, are positive, but they are the vendor's chosen voices, and independent evaluation will matter.

## Conclusion

AlphaGenome 2 is a meaningful refinement for researchers who care about which cell type a variant acts in. Its strongest evidence is the improvement at high precision on single-cell eQTL benchmarks, and its caveats are cost, partial recall and research-only status. Genomics groups, statistical geneticists and drug-discovery teams working from GWAS signals are the natural audience. Anyone who needs genome-wide scores at the lowest cost may still prefer AlphaGenome 1 and the Atlas.

## Editor's Verdict

AlphaGenome 2 Review: DNA Variant Effects by Cell Type earns a solid recommendation within the research space.

The strongest case for paying attention: a conditional design lets one model resolve closely related cell types that earlier bulk-trained models blurred together. That alone raises the bar for what readers should expect in this space. Reinforcing that, its recall at 90% precision on causal TenK10K single-cell eQTLs nearly doubles (30% versus 16%), the regime where variant prioritization needs reliability — practical value rather than just headline appeal. The broader signal worth registering is straightforward: the model treats a tissue, cell type or cell state as an input condition rather than one of thousands of fixed output tracks, which is the core architectural change from the first AlphaGenome. On the other side of the ledger, one constraint is real rather than a marketing footnote: the model needs a separate prediction for each condition, so it is more expensive to run than its predecessor. It should factor into any serious decision. Layered on top of that, at high precision it still recovers only a minority of fine-mapped causal variants, and the overall precision-recall gain is smaller than the headline recall figure suggests — which narrows the set of teams for whom this is an obvious yes.

For ML researchers, technical leads, and readers tracking the underlying science behind new capabilities, this is a serious evaluation candidate, not just a curiosity to bookmark.

### Pros

- A conditional design lets one model resolve closely related cell types that earlier bulk-trained models blurred together
- Its recall at 90% precision on causal TenK10K single-cell eQTLs nearly doubles (30% versus 16%), the regime where variant prioritization needs reliability
- Researchers can condition the model on their own single-cell RNA-seq data without retraining or fine-tuning
- Precomputed enhancer-gene links and variant scores for about 7 million variants spare most labs large-scale computation

### Cons

- The model needs a separate prediction for each condition, so it is more expensive to run than its predecessor
- At high precision it still recovers only a minority of fine-mapped causal variants, and the overall precision-recall gain is smaller than the headline recall figure suggests
- Transient, stimulated and disease cell states are underrepresented in training data and in the precomputed GWAS panel
- It is for research use only and has not been validated or approved for clinical or diagnostic purposes

### References

### Comments0

### Key Insights

- The model treats a tissue, cell type or cell state as an input condition rather than one of thousands of fixed output tracks, which is the core architectural change from the first AlphaGenome.
- The near-doubling headline is recall at 90% precision on TenK10K (30% versus 16%), while overall AUPRC rises more modestly from 0.72 to 0.76.
- On splicing, chromatin accessibility, DNase and polyadenylation QTL benchmarks, the paper reports performance comparable to AlphaGenome 1, so the gains are concentrated in cell-type-specific expression effects.
- Expression conditioning from a few dozen cells lets the model handle unseen cell states, such as vaccine-activated T cells, without fine-tuning.
- Per-condition prediction is more expensive than AlphaGenome 1, which The Verge reports would have made the genome-wide Atlas impractical to build with it.
- The ETS2 and COVID-19 vaccine examples are retrospective reproductions or nominations of biology, not newly validated discoveries.
- The paper acknowledges that condition embeddings reflect correlation rather than causation and that disease and stimulated states are underrepresented in training data.
- Access is limited to non-commercial research through the API for now, with a private preview for commercial users and a research-only, not-clinically-validated disclaimer.

## Related AI Reviews

### Apple Health AI Study Seeks Up to 200,000 iPhone Users

Apple and OpenEvidence open a US research study of up to 200,000 participants on how people use clinically grounded health AI. Not a product launch.

### METR: Anthropic Models Improving at About 2x Historical Rate

METR's 11-week review at Anthropic estimates well-specified capability gains run near 2x the earlier rate, with high uncertainty and heavy redactions.

### OpenAI Says AI Disproved the Exponential Time Hypothesis

OpenAI says its AI disproved the ETH and a Lean check passed. An outside group led by Impagliazzo, Paturi and Williams will verify it.

### NVIDIA Flora: Open-Weight Robot Video Labeling Models

NVIDIA's non-commercial Flora models caption, segment and score robot video; results against GPT-6 Astra are mixed and costs are projections.
