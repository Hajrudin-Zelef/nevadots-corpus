---
id: collect-261121-veille-ia/gemini-google/google-deepmind-s-alphagenome-2-predicts-how-dna-changes-affect-cell-types
title: "Google DeepMind's AlphaGenome 2 predicts how DNA changes affect cell types"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "google", "deepmind", "model", "affect", "human", "types"]
source: https://unicornapi.net/blog/google-deepmind-alphagenome-2-predicts-dna-changes-cell-types
source_anchor: ""
source_lines: [1, 131]
section: "Gemini_Google"
---
# Google DeepMind's AlphaGenome 2 predicts how DNA changes affect cell types

Unicorn News Desk ·

## Key facts

- Maker
- Google DeepMind
- Announced
- 11 November 2026
- Training data
- More than 100 million individual cell profiles across 68 tissues
- Input
- A million-letter DNA sequence plus one biological condition
- Headline claim
- Nearly twice as many variants that change gene expression found, versus the previous model

Google DeepMind on 11 November announced AlphaGenome 2, a model that predicts how DNA sequences and their variants affect specific human cell types rather than whole tissues. The company says it finds nearly twice as many variants that change gene expression in cells as its previous model, and it is available now for non-commercial research through the AlphaGenome API.

The question it targets is a basic one in genetics. Every cell in the body carries the same DNA, yet a heart muscle cell and an immune cell fighting an infection switch different genes on and off. Google DeepMind framed the problem this way: how can a single-letter change affect disease in some cells while sparing the rest? As The Verge puts it, the same genetic variant can have very different effects depending on the type and state of the cell.

## What Google DeepMind released with AlphaGenome 2

The announcement is credited to Adam Gayoso, Natasha Latysheva, Pushmeet Kohli and Žiga Avsec, and it comes with a research paper. Google DeepMind describes AlphaGenome 2 as its next-generation DNA sequence model. It says the model bridges single-cell biology and human genetics by predicting how genetic variants affect hundreds of human cell types.

The model was trained on more than 100 million individual cell profiles across 68 tissues. In more detail, Google DeepMind says the training mixed bulk and single-cell data from human and mouse samples, including over 107 million individual cell profiles. A “cell profile” here is a measurement of molecular activity taken from one cell. “Bulk” data averages that activity across many cells in a tissue sample.

The company says the model “turns static cell atlases into a predictive engine for human genetics”. Cell atlases are large catalogues of individual cells, such as those built by global efforts like the Human Cell Atlas, which is mapping gene expression for millions of cells.

| Spec | AlphaGenome (original) | AlphaGenome 2 | 
|---|---|---|
| DNA input | 1-million-letter sequence | 1-million-letter sequence plus a target biological condition | 
| Output style | Fixed parallel outputs for bulk tissues and cell lines | One condition (tissue, cell type or state) at a time | 
| Readouts | Gene expression, histone modifications, chromatin accessibility and others | The same core readouts, plus DNA methylation | 
| Training data | Not detailed in the sources | More than 100 million cell profiles across 68 tissues, human and mouse | 
| New cell states | Not described | Adapts to new single-cell data without retraining | 
| Access | Not detailed in the sources | AlphaGenome API for non-commercial research. Private preview for commercial research | 

## How AlphaGenome 2 differs from the first AlphaGenome

Google DeepMind launched the first AlphaGenome last year to predict how DNA variants affect molecular activity across broad tissues. It later added AlphaGenome Atlas, a platform with predictions for all 9 billion possible single-letter changes in the human genome. The Verge reports that the Atlas was released in September.

The original model took a million-letter DNA sequence and predicted properties across thousands of experiments from many tissues at once. Google DeepMind says this parallel approach was efficient, but it “spread the model’s capacity thin.” It also made subtle differences between closely related cell types hard to capture.

AlphaGenome 2 changes the design. It is a **conditional model**: it predicts DNA effects for one condition at a time, where a condition is a tissue, a cell type or a cell state. The target condition is fed in alongside the million-letter DNA sequence at every stage of the architecture. That lets the model learn how the same stretch of DNA is read differently across cell types. The condition can come from a reference catalogue of cell types. It can also be generated on the fly from a scientist’s own single-cell experiment, using what Google DeepMind calls a cell encoder.

The model predicts the same core readouts of DNA function as before: gene expression, histone modifications (chemical tags on the proteins DNA wraps around) and chromatin accessibility (how open a stretch of DNA is to the cell’s machinery). It adds DNA methylation, which the company calls “a widely requested readout”.

The conditional design also enables what Google DeepMind calls cross-modality imputation, meaning the model can fill in missing experimental data. Suppose a scientist supplies one measurement, such as single-cell gene activity. AlphaGenome 2 can then predict properties that were not measured, such as chromatin accessibility. The company says this can help researchers decide which experiments to run next.

## Adapting to new lab data without retraining

The feature Google DeepMind stresses most for working scientists is that AlphaGenome 2 adapts to new single-cell data without being retrained. The company says researchers can plug in data from just a few dozen cells to see how DNA changes behave in specific situations. Its examples are:

- immune cells responding to a vaccine
- specific cells in the gut driving inflammatory disease
- stem cells maturing into blood cells

All three examples come from Google DeepMind’s own post.

Sarah Teichmann, co-founder of the Human Cell Atlas consortium and Professor of Stem Cell Medicine at the University of Cambridge, is quoted in the announcement:

What makes AlphaGenome 2 exciting is how it adapts directly to new single-cell data without retraining, predicting how DNA variants act in newly profiled cell states from even a small sample of cells

That quote appears in Google DeepMind’s own post. The Verge also reports that the researchers found the model could make predictions about new cell types and biological states without retraining.

## What Google DeepMind’s own tests show

All performance figures below come from Google DeepMind’s own evaluation. The sources contain no independent benchmark.

**Variant effects in immune cells.** Google DeepMind tested the model on TenK10K, a large single-cell map of 28 primary immune cell types. There, it says AlphaGenome 2 identified 30% of causal variants, against 16% for the comparison methods. It identified 43% of directional gene effects, against 23%. The comparison was against the original AlphaGenome fine-tuned on single-cell data and against “other specialized tools”. A causal variant is a DNA change that actually drives a difference in gene activity. This is distinct from one that merely travels alongside such a change.

**Differences between cell types.** Google DeepMind also tested on genome regions outside the training data. It says AlphaGenome 2 explains significantly more of how gene expression differs between cell types within one tissue. The company says the model also tracks continuous changes in gene expression, such as when human blood stem cells mature into red blood cells and immune lineages.

Overall, the company calls AlphaGenome 2 “a step-change improvement in variant predictive performance”. It says the model can predict variant effects in disease-relevant cells that broad-tissue models would miss.

## How to access AlphaGenome 2

Google DeepMind says AlphaGenome 2 is available from 11 November for non-commercial research via the AlphaGenome API. Commercial research use is in private preview; The Verge describes that preview as limited. The company also points researchers to interactive Colab tutorials. Colab is Google’s hosted notebook environment.

The sources do not give API pricing, rate limits, or terms for the commercial preview. They also do not say whether model weights will be released.

## What it means for builders, and the limits

This is a specialist scientific model, not a general-purpose one. Its users are mainly computational biologists, genomics teams and companies doing drug-discovery research. For developers building tools in that space, the conditional design is the practical change. A tool can now ask a narrow question, such as what a given variant does in one immune cell type, instead of reading a broad tissue average.

Google DeepMind’s genomics lead Žiga Avsec explained to The Verge why that matters for medicine: “When you develop drugs, you’re targeting specific proteins and specific cell types”. Adam Gayoso, a research scientist on the project, told The Verge the model lets researchers drill down on variants found in large populations and see what they may do at the cellular level. He described its significance as “tying human genetics to cell biology”.

The sources raise several limits:

- **Cost.** Avsec told The Verge that AlphaGenome 2’s more detailed predictions are “more expensive” to produce than its predecessor’s. That would have made it impractical to use for the genome-wide Atlas covering every possible DNA letter change.
- **It does not replace the original.** Avsec sees AlphaGenome 2 as a successor, but The Verge reports it is not replacing AlphaGenome any time soon. Builders who need genome-wide coverage may still rely on the first model and its Atlas.
- **Data gaps.** Google DeepMind is working to generate more data, including for cell states that are not well represented today. Predictions for rare or poorly profiled cell states may be less reliable, though the sources give no figures on this.
- **Self-reported results.** The TenK10K figures and the claim of nearly twice as many variants found come from Google DeepMind itself. Independent replication is not yet reported.
- **Commercial access.** Companies cannot simply sign up for commercial use. That route is a private preview.

Avsec also told The Verge that a model like this has only recently become possible. New technologies can now capture individual cells and their states at high resolution, producing the large datasets needed for training.

## What to watch next

Avsec said future versions could be trained on data “across multiple species”. Longer term, he said models like AlphaGenome 2 could feed into efforts to build “virtual cells,” digital simulations designed to mimic biological systems. The Verge also reports that Pushmeet Kohli, who heads Google DeepMind’s AI for science research, has hinted at more scientific results to come.

The near-term points to watch:

- independent groups testing AlphaGenome 2 on their own single-cell data
- whether the commercial private preview opens more widely
- whether Google DeepMind publishes cell-type-level predictions at scale, as it did with the Atlas for the first model

For more of Google DeepMind’s recent research work, see its AI agents formally verifying Signal’s encryption code and the launch of Gemini Robotics 2.5 and an open robot safety benchmark.

## Frequently asked questions

### What is AlphaGenome 2?

AlphaGenome 2 is a DNA sequence model from Google DeepMind, announced on 11 November 2026. It predicts how DNA sequences and their variations affect specific human cell types, such as heart muscle cells or immune cells.

### How is AlphaGenome 2 different from the original AlphaGenome?

The original model predicted outputs for many tissues and cell lines in parallel. AlphaGenome 2 is a conditional model: it predicts for one tissue, cell type or cell state at a time and takes that condition as an input. It also adds DNA methylation as a readout. Google DeepMind says it finds nearly twice as many variants that change gene expression.

### Can I use AlphaGenome 2 through an API?

Yes, for non-commercial research. Google DeepMind says AlphaGenome 2 is available through the AlphaGenome API and interactive Colab tutorials. Commercial research use is in a private preview.

### Does AlphaGenome 2 replace the original AlphaGenome?

Not soon, according to Google DeepMind genomics lead Žiga Avsec, who spoke to The Verge. He said AlphaGenome 2's more detailed predictions are more expensive to produce. That made it impractical to use for the genome-wide AlphaGenome Atlas.

### Are AlphaGenome 2's results independently verified?

The performance figures, including the TenK10K immune-cell results, come from Google DeepMind's own announcement and evaluation. The sources report no independent benchmark yet.
