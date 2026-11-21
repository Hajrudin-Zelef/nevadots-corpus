---
id: collect-261121-veille-ia/gemini-google/google-deepmind-releases-alphagenome-2-to-predict-how-dna-changes-affect-specific-cell-types-and-dis
title: "Google DeepMind releases AlphaGenome 2 to predict how DNA changes affect specific cell types and disease"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "deepmind", "model", "gene", "single-cell", "google", "changes"]
source: https://mlq.ai/news/google-deepmind-releases-alphagenome-2-to-predict-how-dna-changes-affect-specific-cell-types-and-disease/
source_anchor: ""
source_lines: [1, 52]
section: "Gemini_Google"
---
# Google DeepMind releases AlphaGenome 2 to predict how DNA changes affect specific cell types and disease

- AlphaGenome 2 takes a DNA sequence of up to a million letters plus a chosen tissue, cell type or cell state, and predicts gene expression, chromatin accessibility, histone marks and DNA methylation for that condition <sup>[1]</sup> .
- On the TenK10K map of 28 immune cell types, it identified the causal variant 30% of the time versus 16% for the best comparison model, and got the direction of the gene effect right 43% versus 23% <sup>[1]</sup> .
- It was trained on more than 107 million individual cell profiles from human and mouse data, and can condition on a scientist's own single-cell experiment, with as few as a few dozen cells, without retraining <sup>[1]</sup> .
- Non-commercial researchers get it via the AlphaGenome API from today; commercial research is in private preview, and the model is not validated for clinical or diagnostic use <sup>[1]</sup><sup>[2]</sup> .
- Predictions cost more compute than the original AlphaGenome, which will stay in service; DeepMind's genomics lead Žiga Avsec said that made a genome-wide atlas with AlphaGenome 2 impractical <sup>[2]</sup> .

Google DeepMind on Wednesday released AlphaGenome 2, a DNA model that predicts how a genetic variant changes gene activity in a specific human cell type or state, such as a gut macrophage or an activated T cell, rather than in a tissue average <sup>[1]</sup>. DeepMind says it identifies nearly twice as many variants that change gene expression in cells as its previous state-of-the-art model <sup>[1]</sup>.

The model is available from today for non-commercial research through the AlphaGenome API, with commercial research access in private preview <sup>[1]</sup><sup>[2]</sup>. DeepMind also published precomputed scores for trait-associated variants across hundreds of human cell and tissue conditions, and says "AlphaGenome 2 Skills" are coming to Google Antigravity, its agentic development platform <sup>[1]</sup>.

## Why cell type matters for non-coding variants

Almost every cell in the body carries the same DNA, but heart, skin and immune cells switch different genes on and off, and those patterns shift as a cell changes state <sup>[2]</sup>. Most variants that population studies link to disease sit outside genes, in regulatory stretches of DNA that act like dimmer switches for nearby or distant genes. The same variant can therefore do nothing in one cell type and change a gene's output sharply in another <sup>[2]</sup>.

The first AlphaGenome, previewed in June 2025 and published in Nature in January 2026, produced fixed parallel outputs for bulk tissues and cell lines <sup>[1]</sup><sup>[3]</sup>. DeepMind says that design "spread the model's capacity thin" <sup>[1]</sup>. AlphaGenome 2 instead takes a biological condition as an input alongside the DNA sequence and predicts for one condition at a time, using that condition at every stage of the architecture <sup>[1]</sup>. Its readouts are those of the first model (gene expression, histone modifications and chromatin accessibility) plus DNA methylation <sup>[1]</sup>.

A cell encoder lets scientists build a condition from their own single-cell experiment. DeepMind says that with a few dozen cells of single-cell RNA data and no retraining, the model can match or beat models trained or fine-tuned directly on the new dataset <sup>[1]</sup>. Given one measurement, such as single-cell gene activity, it can also predict unmeasured properties such as chromatin accessibility <sup>[1]</sup>.

## The benchmarks

DeepMind trained AlphaGenome 2 on over 107 million individual cell profiles from human and mouse bulk and single-cell data, covering 68 tissues <sup>[1]</sup><sup>[2]</sup>. Its headline test used TenK10K, a single-cell map of 28 primary immune cell types, against the original AlphaGenome fine-tuned on single-cell data and other specialised tools <sup>[1]</sup>.

| TenK10K test (28 immune cell types) | AlphaGenome 2 | Best comparison | 
|---|---|---|
| Causal variants identified | 30% | 16% | 
| Direction of gene effect correct | 43% | 23% | 

These are DeepMind's own evaluations, reported in a paper linked from its blog; the company did not name a journal or preprint server for it <sup>[1]</sup>. It also says the model explains significantly more cell-type expression variation on genome regions held out of training, without giving a figure <sup>[1]</sup>.

## Disease examples

In inflammatory bowel disease, a Nature study had traced risk to a variant in a distant regulatory switch that overactivates the ETS2 gene in gut macrophages. DeepMind says AlphaGenome 2 reproduced that result computationally, picked out the causal variant and showed it selectively boosts ETS2 in those macrophages <sup>[1]</sup>.

Using single-cell data from a COVID-19 mRNA vaccine study that it had not seen in training, the model separated vaccine-activated T cells from resting ones and showed that a variant linked to severe COVID-19 disrupts gene activity more strongly in the activated state <sup>[1]</sup>. DeepMind also says its precomputed scores help pinpoint causal variants and cell types for traits from height to type 2 diabetes <sup>[1]</sup>.

"What makes AlphaGenome 2 exciting is how it adapts directly to new single-cell data without retraining," said Sarah Teichmann, co-founder of the Human Cell Atlas consortium and a professor at the University of Cambridge <sup>[1]</sup>. Peter Donnelly, CEO of Genomics Limited, said the company plans to incorporate the model into its Mystra platform <sup>[1]</sup>. DeepMind is collaborating with the Wellcome Sanger Institute to profile more cell types, lineages and perturbed states <sup>[1]</sup>.

## Cost, limits and what comes next

AlphaGenome 2 does not replace its predecessor. Avsec told The Verge its more detailed predictions are "more expensive" to produce, which would have made it impractical for building the genome-wide atlas covering "every possible DNA letter change in the human genome" that DeepMind released in September <sup>[2]</sup>. DeepMind says the model is for research use only and has not been validated or approved for clinical or diagnostic purposes <sup>[1]</sup>. Its announcement does not mention releasing model weights <sup>[1]</sup>.

"When you develop drugs, you're targeting specific proteins and specific cell types," Avsec said <sup>[2]</sup>. He said DeepMind is generating more data for poorly covered cell states, that future versions could train on data "across multiple species", and that such models could feed into longer-term efforts to build "virtual cells" <sup>[2]</sup>. Applications for DeepMind's community spotlight program, which may come with extra API quota and credits, close Nov. 30 <sup>[1]</sup>. The release follows DeepMind's AlphaProof Code results on Signal and its Gemini Robotics 2.5 release this week.

## Companies mentioned

## Further sources

This article was produced by MLQ.ai's AI newsroom and checked against primary sources such as company filings, official releases and court documents. Reports from a single outlet are attributed. Editor: Peter Foy. How we work · Spotted an error? Tell us.

The stories that matter, in one email. Free — unsubscribe anytime.
