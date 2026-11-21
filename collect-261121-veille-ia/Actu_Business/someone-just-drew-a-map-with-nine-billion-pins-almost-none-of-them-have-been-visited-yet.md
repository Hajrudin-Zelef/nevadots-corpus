---
id: collect-261121-veille-ia/actu-business/someone-just-drew-a-map-with-nine-billion-pins-almost-none-of-them-have-been-visited-yet
title: "Someone Just Drew a Map With Nine Billion Pins. Almost None of Them Have Been Visited Yet."
domain: actu-business
role: reference
task: article
actors: []
dates: ["2026-09-08"]
keywords: ["been", "them", "billion", "score", "just", "nine", "pins", "alphagenome"]
source: https://medium.com/@banerjeevictor06/someone-just-drew-a-map-with-nine-billion-pins-almost-none-of-them-have-been-visited-yet-05838fc842d9
source_anchor: ""
source_lines: [1, 79]
section: "Actu_Business"
---
# Someone Just Drew a Map With Nine Billion Pins. Almost None of Them Have Been Visited Yet.

*AlphaGenome Atlas predicts what every possible single-letter change in human DNA might do. Here is how to read it, what it has already helped find, and why the legend matters as much as the map.*

Somewhere in a child’s DNA, there may be one change that matters.

Doctors suspect a genetic cause. Sequencing returns a long list of differences, thousands of candidates, nearly all of them harmless. The task is to find the one that explains the illness. It is a bit like proofreading a library for a single wrong letter, when you aren’t sure there is a wrong letter at all.

Testing each candidate in the lab is out of the question. So, for most letters, the honest answer to “what does this change do?” has been “we aren’t sure.”

On September 8, 2026, Google DeepMind published its attempt to replace the shrug with a map.

## How to read this map

The human genome is written in about three billion letters. Each letter could be swapped for any of the three others. Multiply those out and you get roughly nine billion possible single-letter changes, which is the number of pins on this map. (The count is measured against the standard human reference genome, and the Atlas also scores more than 100 million insertions and deletions that have been observed in large biobanks.)

The new resource, called AlphaGenome Atlas, holds a prediction for every one of them. DeepMind describes it as a dataset of about one petabyte. It is free to use for academic and other non-commercial research through a website portal, with commercial access on Google Cloud described as coming soon.

Each pin carries a lot. According to the announcement, every variant comes with thousands of predicted molecular effects, spanning hundreds of human and mouse cell types and tissues: whether a stretch of DNA becomes more or less accessible, whether a gene’s splicing goes astray, and so on.

The terrain matters too. Only about 2 percent of the genome codes directly for proteins. The other 98 percent is non-coding. It includes the regulatory switches that help orchestrate when and where genes turn on, though not all of it is known to do anything, and DeepMind says it houses most of the variants linked to traits. That is the territory the AVI score, described below, is designed to cover as well.

## The one-number trick

Thousands of predictions per pin is a lot to read. So the team added a summary: the AlphaGenome Variant Impact score, or AVI.

It draws on two of DeepMind’s models. AlphaGenome predicts what a change does to gene regulation. AlphaMissense predicts what a change does to the protein itself. In the preprint, the AVI score is a trained model that takes 18 inputs, including those two plus conservation and protein-truncation annotations, and condenses them into a single number so researchers can rank variants quickly. It works for both the protein-coding and non-coding parts of the genome. One subtlety: it was trained to tell common variants from rare ones, using rarity as a stand-in for impact, not to match confirmed diagnoses directly.

Then comes the clever part. The score doesn’t just sit there as a verdict. Each one is linked to feature attributions that show which kinds of evidence are driving it inside the model, such as chromatin accessibility, splicing and conservation. A number tells you where to look. The attribution tells you what the score is leaning on, which is a clue to the biology and not a proof of it.

DeepMind says its own testing shows the AVI score performs best in class across many variant-pathogenicity and rare-disease benchmarks. That is DeepMind’s claim, reported in a preprint with outside co-authors that has not yet been peer reviewed, and independent benchmarking will matter. The underlying AlphaGenome model has been through peer review: its paper in *Nature* reports that it matched or exceeded the strongest external models in 25 of 26 variant-effect evaluations. That result is about AlphaGenome itself, not about AVI.

## Three expeditions

A map earns its keep once someone uses it. DeepMind reports three early expeditions, carried out with outside collaborators who are also co-authors of the Atlas paper.

**The needle in the haystack.** Laura Covill and Anne O’Donnell-Luria at the Broad Institute, working with colleagues in the GREGoR rare-disease consortium, used the AVI score to prioritize variants in unsolved cases that earlier work had overlooked. One led to a variant in a gene called *DNM1*, which is strongly linked to epileptic encephalopathy, a group of severe early-onset epilepsy disorders.

The predictions did more than flag the variant. They suggested how it worked: by creating a brain-specific splice site, which leads to an abnormal extension of the resulting protein. According to the announcement, experimental screens then validated the prediction and turned up nearby variants with similar effects. In the paper, that screen was a splicing-reporter assay in cell lines, which tested the predicted splicing effect, and the authors combined it with earlier published case reports to recommend a likely-pathogenic classification for the variant.

**The crowd.** Gareth Hawkes, a Medical Research Council fellow at the University of Exeter, applied the Atlas to whole-genome data from more than 54,000 UK Biobank participants, alongside measurements of about 2,000 proteins circulating in their blood. Rare non-coding variants are notoriously hard to study because most are harmless and drown out the signal. By grouping rare variants according to their predicted molecular effects, Hawkes found 22 percent more significant associations between groups of rare non-coding variants and protein levels than the same analysis without Atlas features, according to the paper. They included variants associated with levels of circulating proteins such as PLA2G7, linked to aging, and EGLN1, a cellular oxygen sensor.

In a second analysis, he looked at body mass index. Focusing on the 1 percent of non-coding variants Atlas predicts to be most impactful, he identified 19 genetic regions, which the announcement describes as leads for the next stage of targeted research.

**The vocabulary.** The Atlas also catalogs more than 2,500 recurring DNA sequence motifs, short stretches that work like words in the genome’s regulatory language. Julia Zeitlinger and Melanie Weilert at the Stowers Institute used them to sort out which transcription factors only affect how accessible DNA is, and which can also turn genes on and off.

## The legend

Here is where a good map reader slows down.

Every pin on this map begins as a prediction. That is a different thing from a measurement, and the three expeditions above sit at very different distances from the ground.

The *DNM1* story reaches the solid dot: a predicted splicing effect checked in an experiment. The UK Biobank work is the half-filled dot. It shows that predictions can sharpen a statistical signal, which is valuable, and which is not the same as showing each flagged variant does what the model says. The 19 body-mass-index regions are, in DeepMind’s own words, a place to direct future research.

And nearly all of the nine billion pins are hollow circles. That is not a flaw in the map. It is simply what a map of nine billion possibilities looks like on day one.

## What the map doesn’t say

A few more cautions belong on the margin.

**It isn’t clinical.** The announcement states plainly that AlphaGenome has not been validated for, and is not approved for, any clinical use. The Atlas is a research tool. Please do not look up your own letters and draw conclusions.

**Most Atlas evidence is still the team’s own.** AlphaGenome itself has been through peer review. The Atlas and AVI claims rest on DeepMind’s announcement and a preprint that has not yet been peer reviewed. The three case studies were carried out with collaborators who are co-authors of that preprint, so they are not independent tests, and the announcement cites no independent replication.

**The 22 percent needs its baseline.** It compares Atlas-filtered groups of rare variants against groups built without Atlas features, across about 2,000 blood proteins, and the preprint notes it used slightly out-of-date Atlas releases. For height, body mass index and a blood-sugar marker, only a few of the associations replicated even nominally in a second cohort, which is why the 19 regions are best read as leads.

**A prediction can be wrong.** Any model’s output will miss sometimes, and a map of nine billion points will contain misses. That is why the experimental checks matter.

## Why draw a map at all?

DeepMind calls the Atlas a baseline rather than an endpoint, and says its maps should improve as the underlying models do. It also compares the project to the AlphaFold Database, which gave researchers with no coding experience a way to browse predicted protein structures at scale.

Whether the Atlas does for the genome what that database did for proteins will be decided by the people who use it, in labs, over the next several years.

For now, here is the useful way to hold it. A map doesn’t tell you where the treasure is. It tells you where to look first, so that the expensive, slow, irreplaceable work of actually going there can be spent on better bets.

Nine billion pins, and a legend that says how many have been visited. That is exactly the right thing to read before you set out.

*Sources: Google DeepMind, “AlphaGenome Atlas: A predictive map of every possible DNA letter change in the human genome” (September 8, 2026), including the figures and collaborator results as reported there; J. Cheng et al., “AlphaGenome Atlas: in silico mutagenesis of the entire human genome improves prioritization and interpretation of non-coding variants,” medRxiv preprint (posted September 20, 2026), not peer reviewed; Ž. Avsec et al., “Advancing regulatory variant effect prediction with AlphaGenome,” Nature 649, 1206–1218 (2026), for the 25-of-26 figure. Atlas and AVI performance claims are DeepMind’s own; no independent replication is cited.*
