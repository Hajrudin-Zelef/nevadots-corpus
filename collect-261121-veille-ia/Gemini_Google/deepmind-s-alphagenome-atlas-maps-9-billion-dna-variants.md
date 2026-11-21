---
id: collect-261121-veille-ia/gemini-google/deepmind-s-alphagenome-atlas-maps-9-billion-dna-variants
title: "DeepMind's AlphaGenome Atlas Maps 9 Billion DNA Variants"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-08"]
keywords: ["atlas", "deepmind", "alphagenome", "model", "billion", "predictions", "gene", "human"]
source: https://edufabtech.com/news/deepmind-alphagenome-atlas-9-billion-dna-variants
source_anchor: ""
source_lines: [1, 50]
section: "Gemini_Google"
---
# DeepMind Publishes Precomputed Predictions for All 9 Billion Possible Human DNA Changes

The AlphaGenome Atlas turns a slow, per-query model into a searchable, petabyte-scale lookup table, and researchers are already using it to find disease-linked variants.

On September 8, 2026, Google DeepMind released the AlphaGenome Atlas, a free, searchable database of precomputed predictions for all 9 billion possible single-letter changes to the human genome. Instead of requiring researchers to run the AlphaGenome model themselves for every variant they want to study, the effects have already been calculated and can be looked up in a browser. The full dataset runs to roughly a petabyte, which Scientific American reports is about 30 times larger than the AlphaFold Protein Structure Database.

The release matters less as a headline number than as a change in who can do this kind of work, and how fast. Genome sequencing has been cheap and fast for years; figuring out what a given DNA change actually does has not. AlphaGenome, the underlying model DeepMind first announced as a preprint in June 2025 and later published in *Nature* in January 2026, predicts how a DNA sequence affects gene regulation by comparing an altered sequence against a reference one across a one-million-base-pair window. Running that comparison for every one of 9 billion positions and folding the results into a searchable atlas is what changed on September 8.

## What the atlas actually contains

For each variant, the Atlas provides thousands of molecular-effect predictions spanning multiple aspects of gene regulation across hundreds of human and mouse cell types and tissues, according to DeepMind's technical announcement. It also introduces a single summary metric, the AlphaGenome Variant Impact (AVI) score, which combines AlphaGenome's regulatory predictions with the older AlphaMissense model's coding-region predictions into one number that works across both the roughly 2% of the genome that codes for proteins and the 98% that does not.

That non-coding majority is the part genomics has struggled with. A single-letter change inside a gene often has an obvious, well-studied consequence. A single-letter change in a regulatory region hundreds of thousands of bases away can silence or amplify a gene without any accepted way, until now, to predict which. DeepMind cites early uses of the underlying model to identify a disease-linked variant near the DNM1 gene associated with epileptic encephalopathy, to recover additional non-coding associations in a UK Biobank analysis, and to help identify genomic regions linked to body mass index, work the announcement credits to outside research groups.

## An 80x speedup, not just a bigger model

The engineering story here is arguably as notable as the biology. IEEE Spectrum reports that DeepMind's team calculated they would need to raise their computation speed by a factor of 80 to compile the Atlas in a workable amount of time, and did so through model distillation, GPU kernel optimization, and elimination of redundant calculations. That is a fairly ordinary systems-engineering problem — how do you make a slow model cheap enough to run at planetary scale — solved in service of a scientific resource, and it is the reason a model published in January 2026 could be turned into an exhaustively precomputed atlas by September of the same year.

Pushmeet Kohli, DeepMind's VP of Science, told Scientific American the release "represents the first time that any researcher in the world can access a comprehensive map of the human genome and its variations by simply opening a browser." Jonathan Sebat, a psychiatric geneticist at UC San Diego, put the practical effect more plainly to the same publication: "Our own workflows in the lab can be streamlined quite a bit because we don't actually have to compute anything. We literally can just look up everything."

## What the atlas does not settle

Independent reviewers were careful to separate the computational achievement from a cure for the underlying biological problem. Carl de Boer, a genomicist at the University of British Columbia, called AlphaGenome "the field's leading model" but told IEEE Spectrum it remains "very slow and computationally intensive" in its unoptimized form, and warned that reducing a variant's effects to a single AVI score "is probably going to be easily misinterpreted" given how much biological complexity that number compresses. He also pointed out that many diseases arise from combinations of variants rather than single changes, and that some regulatory sequences, particularly distant enhancers, act beyond the model's one-million-base-pair field of view — so the Atlas answers "what does this one change do in isolation" rather than "what does this patient's genome do."

DeepMind's own documentation states the resource "has not been validated for, and is not approved for, any clinical use," and is not a substitute for professional medical diagnosis. Access is free for non-commercial research through a web portal and the AlphaGenome API on GitHub, but Scientific American notes that, unlike the AlphaFold database, commercial users such as drug developers will need a paid license to use it — a distinction that will shape how quickly pharmaceutical research groups, as opposed to academic labs, can build on the resource.

## Why this belongs in a computing story, not only a biology one

The Atlas is a useful marker of where large predictive models are heading once they mature past their initial release: from a tool a lab runs on demand to infrastructure a field builds on top of, the same trajectory the AlphaFold Protein Structure Database took after 2021. The distillation and kernel-optimization work that made the 80x speedup possible is generic systems engineering that will likely resurface in the next model DeepMind or a competitor tries to turn into a queryable, internet-scale resource, in genomics or elsewhere. Readers can follow ongoing coverage of AI-for-science releases like this one in EduFabTech's news section.

A quick question for readers

Everyone has a prediction. Add yours and see what students are betting on.

Pick an answer, create a free account in a minute, and your vote counts. Already a member? Sign in

## Sources (3)

1. 
            Google DeepMind.             **AlphaGenome Atlas: Molecular predictions for 9 Billion human DNA variants** .
            Google DeepMind, 2026.
                          deepmind.google ↗
2. 
            Google DeepMind.             **AlphaGenome: AI for better understanding the genome** .
            Google DeepMind, 2025.
                          deepmind.google ↗
3. 
            Žiga Avsec et al..             **Advancing regulatory variant effect prediction with AlphaGenome** .
            Nature, 2026.
                          nature.com ↗
