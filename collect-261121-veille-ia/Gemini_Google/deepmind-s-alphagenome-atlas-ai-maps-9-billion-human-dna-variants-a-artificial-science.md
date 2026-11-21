---
id: collect-261121-veille-ia/gemini-google/deepmind-s-alphagenome-atlas-ai-maps-9-billion-human-dna-variants-a-artificial-science
title: "DeepMind's AlphaGenome Atlas: AI Maps 9 Billion Human DNA Variants â Artificial Science"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-08"]
keywords: ["alphagenome", "deepmind", "atlas", "human", "billion", "genome", "variants", "model"]
source: https://artificialscience.org/2026/09/deepmind-alphagenome-atlas-maps-9-billion-dna-variants/
source_anchor: ""
source_lines: [1, 107]
section: "Gemini_Google"
---
Biologists have spent two decades staring at a book they cannot fully read. The human genome was first drafted in 2001 and largely finished soon after, yet most of its three billion letters still resist interpretation. We know the spelling. We rarely know the meaning.

On Tuesday, Google DeepMind changed the odds. Its new AlphaGenome Atlas hands any researcher a ready-made prediction for what happens when you alter a single letter of human DNA, across every one of the nine billion possibilities, through a free web page. No code to write, no model to run, no supercomputer to book. For a field that normally tests one mutation at a time, that is a different way of working. It also arrives with the familiar DeepMind blend of real promise and bold claims worth checking.

## What DeepMind actually released#

On 8 September 2026, DeepMind introduced AlphaGenome Atlas, describing it as "the most comprehensive catalogue of how genetic mutations affect molecular biology". The team took its existing AlphaGenome model, ran it across every possible single-letter change in a reference human genome, and stored the answers so that nobody has to recompute them.

The arithmetic behind the headline is simple. The genome holds roughly three billion base pairs, and each position can be swapped for one of three other letters, which gives nine billion variants. Precomputing all of them produced a one-petabyte dataset (a million gigabytes), more than thirty times the size of the AlphaFold protein database.

The feature most researchers will reach for first is a shortcut. Alongside the raw predictions, DeepMind released the AlphaGenome Variant Impact (AVI) score, a single number that ranks how disruptive a variant is likely to be. It blends predictions from AlphaGenome with those from AlphaMissense, an earlier DeepMind model for protein-coding mutations, so one score spans both the 2% of the genome that codes for proteins and the 98% that does not. Look up a variant in the browser and you see its AVI score plus a breakdown of which biological processes are predicted to fail, among them RNA splicing, gene expression and the packaging of DNA inside the cell. The Atlas is free for academic use through a website portal and the AlphaGenome API, with commercial access on Google Cloud promised later.

## Background: the genome's "dark matter"#

A little biology makes the release easier to judge. DNA is often described as a recipe book for proteins, the molecular machines that do most of the work in a cell. But the stretches that actually spell out proteins, the genes, make up only about 2% of human DNA. The other 98% was once dismissed as "junk". It is nothing of the sort. Much of it regulates genes, deciding which are switched on, in which tissues, and how loudly. This non-coding region is where a large share of disease-linked genetic variation sits, and it is far harder to read than the coding part.

That interpretation problem is what AlphaGenome was built to attack. Announced in June 2025 and detailed in a *Nature* paper in January 2026, it is a "sequence-to-function" model. Feed it a long stretch of DNA, up to a million letters at single-base resolution, and it predicts thousands of molecular read-outs, such as where genes start and stop, how RNA is spliced, and how accessible the DNA is to the cell's machinery. It can also compare a healthy sequence with a mutated one and estimate what the change does.

Until now, using AlphaGenome meant choosing your variants, writing code and running a heavy model yourself. The Atlas removes those steps by doing the computation in advance for the whole genome. As DeepMind puts it, AlphaGenome was built to study variants one at a time; the Atlas offers a big-picture view across all of them.

## Why this matters#

The clearest payoff is speed for everyday genetics. Jonathan Sebat, a psychiatric geneticist at the University of California, San Diego, told *Scientific American* that his lab's workflows "can be streamlined quite a bit because we don't actually have to compute anything. We literally can just look up everything." When a tool that used to demand a coding project becomes a browser search, far more scientists can use it.

Early collaborators put numbers to that. Working with the GREGoR Consortium, researchers at the Broad Institute used the AVI score to re-rank variants in unsolved rare-disease cases and surfaced one in the *DNM1* gene, which is tied to a severe childhood epilepsy. The model showed the mechanism: the variant creates a faulty splice site that lengthens the resulting protein. Lab experiments then confirmed it. At the University of Exeter, Medical Research Council fellow Gareth Hawkes applied the Atlas to whole-genome data from more than 54,000 UK Biobank participants and found 22% more non-coding genetic associations than standard methods could detect, including regulatory variants affecting proteins linked to ageing and oxygen sensing. Narrowing to the 1% of variants the Atlas rated most impactful, he pinpointed 19 regions of the genome associated with body mass index.

The comparison everyone reaches for is AlphaFold, DeepMind's protein-structure model, whose free database reshaped biology and earned Demis Hassabis a share of the 2024 Nobel Prize in Chemistry. AlphaFold mapped the shapes of proteins; the Atlas maps the likely consequences of DNA changes. If it lands the same way, the long-term effect could reach the hunt for drug targets, the design of genetic studies, and the interpretation of the variants that clinicians already struggle to classify.

## What to watch#

Enthusiasm should come with caveats, and DeepMind's own framing supplies several. Å½iga Avsec, the genomics lead, calls the Atlas a baseline rather than an endpoint: a good place to start the search for a needle in a haystack, not the final answer.

Three limits stand out. First, the Atlas scores one letter at a time, yet many conditions arise from several variants acting together, which single-variant scores cannot capture. Second, the underlying model reads a window of about a million base pairs, and some regulatory elements called enhancers act over even longer distances, beyond its field of view. Third, the convenient AVI score is, by design, a compression of many signals into one figure. Carl de Boer, a genomicist at the University of British Columbia who is not affiliated with DeepMind, told *IEEE Spectrum* it "has a clear use, but it also is probably going to be easily misinterpretedâ¦ we're talking about a very complex system and there's a lot of moving parts."

Accuracy is the deeper question. *Scientific American* notes that the Atlas is vastly larger than AlphaFold's database but far less accurate, and that gap is built in. A protein's 3D shape is a crisp physical target; gene regulation is fuzzy, context-dependent and varies between tissues. A preprint (not peer reviewed) reports that predicting gene activity in a specific individual remains a known weakness of models in this class. And DeepMind is explicit that AlphaGenome has not been validated for, and is not approved for, any clinical use. These are predictions to guide experiments, not diagnoses.

## A genuine step, not a magic bullet#

Independent specialists have been warm but measured. When the AlphaGenome paper appeared in *Nature*, the UK's Science Media Centre gathered reactions. Kristian Helin of the Institute of Cancer Research called it "a major advance" whose immediate impact "may be incremental" while its longer-term significance is "substantial", much as AlphaFold's influence built up over years. Xianghua Li of King's College London was blunter: the model "performs as well as the best existing tools, but not better", and such predictions are "not yet ready for use in clinics". Aldo Faisal of Imperial College London urged treating the results "as promising rather than final" until other groups reproduce them.

So what is new here? Not the model, which existed already, and not a fresh biological discovery. The novelty is scale plus access. DeepMind built on its own lineage (Enformer for gene regulation, AlphaMissense for protein variants, AlphaFold for structure), then did the brute-force work the community kept repeating. Avsec's team reckons it had to speed up AlphaGenome roughly eightyfold, using model distillation and a lot of engineering, to compute nine billion predictions at all. The result spares thousands of labs from running the same simulations and, as de Boer notes, helps researchers who lack access to the newest hardware. That is a quieter contribution than a Nobel-winning breakthrough, but for day-to-day science it may matter just as much.

## Key takeaways#

1. AlphaGenome Atlas precomputes the molecular effects of all nine billion single-letter human DNA variants, a one-petabyte dataset that is over thirty times the size of the AlphaFold database.
2. The new AVI score ranks any variant's likely impact across both coding and non-coding DNA, and shows which processes it is predicted to disrupt.
3. Access is the real change: the resource is free to use in a browser, which removes the coding and compute barriers that limited the original model.
4. Early collaborators used it to help solve an unsolved rare-disease case and to surface 22% more non-coding signals in UK Biobank data.
5. The predictions are noisier than AlphaFold's structures, cover single variants only, and are explicitly not approved for clinical use.

## Frequently asked questions#

**Is AlphaGenome Atlas free?**
Yes, for non-commercial academic research through the website portal and the AlphaGenome API. Commercial access through Google Cloud is listed as coming soon.

**Do I need to code to use it?**
No. The portal lets a researcher look up a variant and read its predicted effects without writing any code, which is the main change from the original model.

**What is the AVI score?**
The AlphaGenome Variant Impact score is a single number summarising how disruptive a variant is predicted to be. It combines AlphaGenome's regulatory predictions with AlphaMissense's protein predictions.

**How is this different from AlphaFold?**
AlphaFold predicts the 3D structures of proteins. AlphaGenome Atlas predicts the molecular consequences of changes in DNA, including the non-coding regions that regulate genes. It is much larger but less accurate.

**Can it diagnose my genetic condition?**
No. DeepMind states the model is not validated or approved for clinical use. It is a research tool for generating and prioritising hypotheses, not medical advice.

**Does it cover every disease-causing mutation?**
No. It scores single-letter changes only. Many conditions involve several variants together, or larger structural changes, which fall outside the Atlas.

**Who built it and where can I read the details?**
The AlphaGenome Atlas team at Google DeepMind, with academic collaborators including the Broad Institute, the University of Exeter and the Stowers Institute. The full methods are in the technical report.

## Glossary#

**Genome:** the complete set of DNA in an organism. The human genome contains about three billion base pairs.

**Base pair:** a single rung of the DNA "ladder", made of two paired chemical letters. A single-nucleotide variant changes one letter of one rung.

**Coding vs non-coding DNA:** coding DNA (about 2%) spells out proteins; non-coding DNA (about 98%) largely regulates when and where genes are active.

**Gene regulation:** the cellular control system that switches genes on or off and tunes their output across different tissues.

**RNA splicing:** the editing step that cuts and rejoins the RNA copy of a gene. Errors can produce faulty proteins.

**Single-nucleotide variant:** a change of one DNA letter to another at a given position. The Atlas scores all nine billion of them.

**Sequence-to-function model:** an AI system that predicts biological activity directly from a DNA sequence, as AlphaGenome does.

**Enhancer:** a regulatory stretch of DNA that can boost a gene's activity, sometimes from a great distance along the genome.


## References#

1. Google DeepMind, AlphaGenome Atlas: A predictive map of every possible DNA letter change in the human genome, 8 September 2026. *Primary announcement.*
2. Google (The Keyword), AlphaGenome Atlas: a high-resolution map of human DNA, by Pushmeet Kohli and Å½iga Avsec, 8 September 2026.
3. Google DeepMind, AlphaGenome Atlas technical report (PDF), 2026.
4. AlphaGenome Atlas portal, alphagenome.google/atlas.
5. Scientific American, New Google DeepMind atlas could transform our understanding of genetic diseases, 8 September 2026.
6. Google DeepMind, AlphaGenome: AI for better understanding the genome, June 2025. *Background on the base model.*
7. Science Media Centre, Expert reaction to paper on Google DeepMind's AlphaGenome, 28 January 2026. *Independent expert commentary on the peer-reviewed* Nature*paper, DOI: 10.1038/s41586-025-10014-0.*
8. Google DeepMind, AlphaMissense: A catalogue of genetic mutations to help pinpoint the cause of diseases, 2023.
9. Google DeepMind, AlphaFold reveals the structure of the protein universe, 2022.
10. GREGoR Consortium, gregorconsortium.org.
11. bioRxiv, AlphaGenome Enhances Personal Gene Expression Prediction but Retains Key Limitations. **Preprint (not peer reviewed).**

*This article is for information only. It does not offer medical or genetic advice. The research described includes predictions from AI models that are not approved for clinical use. Preprints are labelled as such and should not be read as established scientific consensus.*
