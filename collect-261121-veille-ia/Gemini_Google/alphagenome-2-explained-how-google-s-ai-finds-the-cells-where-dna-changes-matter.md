---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-explained-how-google-s-ai-finds-the-cells-where-dna-changes-matter
title: "AlphaGenome 2 Explained: How Google's AI Finds the Cells Where DNA Changes Matter"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "cells", "cell", "matter", "same", "disease", "google", "changes"]
source: https://ai-insights.sitenova.dev/blog/alphagenome-2-google-deepmind-dna-cell-types-research-explained
source_anchor: ""
source_lines: [1, 125]
section: "Gemini_Google"
---
## On this page

## What you’ll learn

- What changed from AlphaGenome to AlphaGenome 2?
- Why the same mutation can matter in one cell and not another
- The headline result—and what the percentages really mean
- A concrete example: inflammatory bowel disease

How AlphaGenome 2 connects DNA variants to specific cells, what its 30% vs 16% result means, and why it is not a disease diagnosis.

**Google DeepMind announced AlphaGenome 2 on November 11, 2026.** It is not an AI that reads your DNA and tells you whether you will become ill. Its more specific—and scientifically important—goal is to predict *how changes in DNA alter gene regulation in particular kinds of cells*. This may help researchers turn a statistical association with disease into a testable biological mechanism.

The distinction matters. Almost every cell in your body contains much the same DNA, yet a macrophage, a heart-muscle cell and a neuron use different parts of that shared instruction set. A genetic variant can therefore be consequential in one cell type and have little detectable effect in another. AlphaGenome 2 is built to ask not only **“What does this DNA change do?”** but also **“In which cells, and in what state?”**

## What changed from AlphaGenome to AlphaGenome 2?

The original AlphaGenome model, announced in 2025, analyzed DNA sequences up to about one million letters long and predicted molecular properties across a fixed collection of tissues and cell lines. That approach was powerful, but combining many cell types into broad tissue-level averages could obscure subtle effects specific to specialized cells.

AlphaGenome 2 still works with roughly one-million-letter DNA sequences. The fundamental architectural change is **conditioning**: researchers provide a target tissue, cell type or biological state alongside the sequence, and the model predicts molecular effects for that condition. In simple terms, instead of asking one giant question about a DNA segment across many tissues, researchers can ask a focused question about that segment in a particular cell population.

DeepMind says the new system was trained using **more than 107 million single-cell profiles spanning 68 tissues**, alongside other genomic data from human and mouse samples. It can predict features including gene expression, chromatin accessibility, histone modifications and DNA methylation. It can also use measurements from a newly profiled set of cells to help predict unmeasured molecular properties, without needing to retrain the entire model on that dataset.

## Why the same mutation can matter in one cell and not another

Think of DNA as a shared cookbook in a large restaurant. The pastry chef, the soup cook and the grill cook own the same book but follow different sections. A smudged instruction in the pastry chapter matters mainly to the pastry station. Likewise, a change in a regulatory DNA region may alter a gene's activity only in cells that use that particular regulatory switch.

This is especially relevant because many variants associated with human traits and diseases lie outside protein-coding genes. These non-coding regions can control *when, where and how strongly* genes are expressed. Predicting a variant's effect without identifying the relevant cells can leave scientists with a long list of plausible explanations and no clear experiment to run next.

## The headline result—and what the percentages really mean

In one evaluation using the TenK10K single-cell dataset, DeepMind reports that AlphaGenome 2 identified **30% of causal expression-linked variants compared with 16% for its predecessor**, and identified directional gene effects at **43% versus 23%**. The associated research preprint describes roughly doubled recall of causal single-cell eQTLs at a **90% precision** setting. These figures refer to specific benchmark tasks and thresholds; they do **not** mean AlphaGenome 2 predicts disease accurately 90% of the time or diagnoses nearly twice as many patients.

An *eQTL* (expression quantitative trait locus) is a DNA variant associated with how much a gene is expressed. Finding an eQTL is useful, but determining whether it causes a disease—and whether changing its action would be safe—requires further evidence. Model performance also varies with the test dataset, the cell type, available training data and the biological question.

## A concrete example: inflammatory bowel disease

One of the strongest illustrations comes from research into inflammatory bowel disease (IBD). Earlier laboratory work identified a regulatory DNA variant associated with increased activity of the **ETS2** gene in gut macrophages, a type of immune cell. DeepMind reports that AlphaGenome 2 reproduced key aspects of this finding computationally: it highlighted the variant and predicted a stronger ETS2-regulatory effect in gut macrophages than in many other examined cells.

Independent reporting in *Nature* quoted immunologist James Lee, whose team conducted the underlying experimental work, describing how the new model recapitulated the answer in about 30 minutes, compared with a research effort that took roughly a year and a half. That is an illuminating example of *retrospective prediction*, not proof the model will routinely replace 18 months of research or discover equally robust mechanisms in unexplored diseases. Laboratory validation remains essential.

## A second advance: learning from unfamiliar cell states

Cells change state when they mature, encounter infection, respond to medication or become diseased. AlphaGenome 2 can use single-cell RNA sequencing measurements as a description of a new cell state, then generate predictions without fine-tuning on that exact dataset. DeepMind describes a case involving vaccine-activated versus resting immune T cells, where the model predicted a stronger regulatory effect for a variant linked to severe COVID-19 in the activated state.

This matters because real biology does not fit a fixed menu of cell labels. If predictions can follow the cell's current state, researchers may better prioritize studies in development, immunology and treatment response. These remain research predictions, not individual treatment recommendations.

## What AlphaGenome 2 could actually help scientists do

1. **Prioritize variants:** Narrow a long list of disease-associated DNA changes to candidates with predicted regulatory effects in biologically relevant cells.
2. **Connect distant enhancers to genes:** Suggest which gene a non-coding regulatory region may control, even when it sits far from that gene.
3. **Choose better experiments:** Select cell types, conditions and variants for CRISPR perturbation, sequencing or other validation before committing laboratory resources.
4. **Interpret new cell atlases:** Use single-cell measurements from previously unseen states to generate hypotheses about additional molecular properties.

Consider a *hypothetical* autoimmune-disease project: genetic association data implicates a region containing multiple variants. A team could compare AlphaGenome 2 predictions in resting and activated immune cells, shortlist candidate variants and target genes, and then experimentally test the predictions. This is a proposed workflow, not a study performed by AI Insights.

## What it cannot tell you

**It is not a clinical diagnostic test.** DeepMind explicitly states that AlphaGenome and AlphaGenome 2 are research tools and have not been validated or approved for clinical or diagnostic purposes. A model prediction that a variant affects gene expression does not determine whether a particular person will develop cancer, IBD or another disease.

Biological outcomes depend on many additional factors: other genetic variants, environment, cell-to-cell interactions, development, time and the difference between an observed molecular change and a clinically meaningful effect. Benchmark success also does not establish that the model generalizes equally well to every ancestry, rare cell population or clinical context. Researchers should inspect uncertainty and seek independent experimental validation rather than treating the model's preferred hypothesis as a proven cause.

## How to evaluate AlphaGenome 2 responsibly

For a research lab, a useful evaluation would begin with a **held-out set of variants and cell types whose experimental effects are known**. Compare AlphaGenome 2 against the previous model and a simple biological baseline. Report precision and recall at clearly specified thresholds, direction-of-effect accuracy, performance by cell type and the fraction of predictions that survive new wet-lab experiments. Crucially, separate retrospective reproduction of known findings from genuine prospective discoveries.

And compare resource use as well as accuracy: the cell-conditional method may require repeated evaluations across many target states, so a detailed cellular analysis can cost more computation than broad screening. The strongest workflow may combine a cheaper initial filter with AlphaGenome 2 for targeted follow-up.

## Can researchers access it now?

As of **November 13, 2026**, DeepMind says AlphaGenome 2 is available through the **AlphaGenome API for non-commercial research**, with tutorials for expression conditioning. **Commercial research access is in private preview**, not unrestricted general availability. DeepMind also says an AlphaGenome 2 integration for Google Antigravity is planned; a future integration should not be described as already live.

## The bigger implication

Many genetics studies can tell us *where* disease-associated DNA differences are found. What researchers often need next is a more precise explanation: **which variant changes which gene, in which cell, under which condition?** AlphaGenome 2 offers a promising computational bridge between genomic association studies and cell biology. Its real value will be measured not by headline AI benchmarks alone, but by how often its predictions lead to independently confirmed mechanisms and better-targeted experiments.

**Bottom line:** AlphaGenome 2 is best understood as a tool for generating sharper biological hypotheses—not as an AI doctor, a replacement for experiments or proof of new treatments.

## Sources and editorial method

- Google DeepMind, “AlphaGenome 2: Deciphering how cells read DNA to better understand disease” (November 11, 2026): official announcement, capabilities, benchmarks, access and clinical disclaimer.
- Gayoso et al., “AlphaGenome 2 predicts regulatory activity and variant effects across cell states” (research preprint): methodology and benchmark framing.
- Ewen Callaway, “DeepMind’s ultra-powerful AI for genome mapping wows scientists,” Nature (November 11, 2026): independent expert reporting and discussion of the IBD example.
- Google DeepMind, original AlphaGenome announcement (June 25, 2025): background on the prior model.

*Editorial disclosure: This article is an original explanatory synthesis of public primary research and independent reporting checked November 13, 2026. AI Insights did not run AlphaGenome 2, reproduce its experiments or validate its biological predictions independently. All hypothetical examples are labelled.*

## Was this article helpful?

## About the author

Kavish Ganatra

Kavish works on SiteNova websites and the AI Insights publishing workflow, with a focus on making AI automation easier to understand and use.

Editorial standards →·Suggest a correction
## Keep reading

### AI Found a Possible New Antibiotic in Millions of Proteins. Now Comes the Hard Part.

Google's AI-assisted protein search found a potential antimicrobial peptide. Here's how it works, the evidence, and why lab testing still matters.

ai agents · ai research · applied ai

For beginners, developers, professionals

Read article
### AI Claims Three Major Scientific Breakthroughs. Who Checks the Proofs?

OpenAI claims breakthroughs in Kakeya, complexity theory and quantum field theory. Here's what the proofs claim and why independent review matters.

ai models · ai research · formal verification

For beginners, developers, professionals

Read article
### Thousands of AI Agents Studied Mouse Brain Cells in 32 Minutes. What Did They Find?

Thousands of AI agents analyzed mouse brain-cell data in 32 minutes. Explore the reported hypotheses and the scientific limits of the work.

ai agents · ai research · neuroscience

For beginners, developers, professionals

Read article
