---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-explained-deepmind-s-cell-type-dna-variant-model
title: "AlphaGenome 2 explained: DeepMind's cell-type DNA variant model"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "model", "cell", "variants", "deepmind", "gene", "variant", "deepminds"]
source: https://promptandpower.com/alphagenome-2-deepmind-cell-type-dna-variants/
source_anchor: ""
source_lines: [1, 31]
section: "Gemini_Google"
---
Most of the genetic variants linked to common diseases don’t sit inside genes. They sit in the stretches of DNA that switch genes on and off, and they often matter only in one kind of cell. Google DeepMind’s AlphaGenome 2, announced Nov. 11, is built to predict exactly that: what a change in DNA does in a specific human cell type.

## What’s new

The first AlphaGenome, released in 2025, read up to a million letters of DNA and predicted how variants affect gene regulation. AlphaGenome 2 is what DeepMind calls a conditional model. You give it the same million-letter stretch of DNA plus a target, such as a tissue, a cell type or a cell state, and it predicts the effects in that one setting.

It was trained on more than 107 million individual cell profiles from human and mouse data, across 68 tissues, according to DeepMind’s technical description. It can also infer properties that were never measured in a sample, such as predicting which DNA is accessible from a cell’s gene activity alone.

## The numbers DeepMind reports

On TenK10K, a dataset covering 28 types of primary immune cells, AlphaGenome 2 identified likely causal variants 30% of the time, compared with 16% for the original AlphaGenome fine-tuned on single-cell data and other specialised tools. It got the direction of a variant’s effect on a gene right 43% of the time, against 23%. Overall, DeepMind says, it finds nearly twice as many variants that change gene expression as the previous best model.

As a check, the team says the model reproduced a known result in inflammatory bowel disease: a variant that boosts the ETS2 gene in gut macrophages. It also predicted variant effects in a cell state it hadn’t been trained on, activated T cells from a COVID-19 mRNA vaccine study, without retraining.

These are DeepMind’s own results. Independent groups haven’t yet published comparisons.

## Who can use it, and what it isn’t

AlphaGenome 2 is available for non-commercial research through the AlphaGenome API, with a private preview for commercial research. DeepMind’s post doesn’t mention releasing model weights or list a price, though it says some community projects may qualify for extra quota and credits. “AlphaGenome 2 Skills” for Google’s Antigravity coding tool are described as coming soon.

DeepMind is explicit that both AlphaGenome models are for research only and have not been validated or approved for clinical or diagnostic use. That matters: a model that ranks candidate variants for a lab to test is a long way from one that tells a patient what their genome means.

## Why it matters

For researchers, the gain is triage. Genome-wide studies throw up thousands of suspect variants; doubling the hit rate on which ones are causal, in the right cell, could save years of bench work. The Wellcome Sanger Institute is collaborating on profiling more human cell types, and Genomics Limited says it plans to build the model into its Mystra platform.

For everyone else, the honest summary is that this is better research infrastructure, not a new medical test. The test comes later, if the predictions keep holding up in the lab.

**More from prompt/power:** Australia’s chatbot rules for children · Canada’s Budget 2026 AI spending · Multi-agent teams do worse together

Researched and drafted with AI tools, fact-checked against primary sources and approved by a human editor before publishing. Prices and dates are as of publication. Our standards →
