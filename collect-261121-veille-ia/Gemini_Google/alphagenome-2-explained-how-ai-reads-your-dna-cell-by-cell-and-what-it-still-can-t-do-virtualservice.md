---
id: collect-261121-veille-ia/gemini-google/alphagenome-2-explained-how-ai-reads-your-dna-cell-by-cell-and-what-it-still-can-t-do-virtualservice
title: "AlphaGenome 2 Explained: How AI Reads Your DNA Cell by Cell, and What It Still Can't Do - VirtualServicess"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "alphagenome", "model", "same", "explained", "condition", "type", "does"]
source: https://virtualservicess.com/alphagenome-2-explained-how-ai-reads-your-dna-cell-by-cell-and-what-it-still-cant-do/
source_anchor: ""
source_lines: [1, 103]
section: "Gemini_Google"
---
Every cell in your body carries the same DNA. That is the strange part. A skin cell and a brain cell hold the same instruction book, yet they read completely different pages. Google CEO Sundar Pichai put it simply when Google DeepMind announced its newest model on 11 November 2026: every cell shares the same DNA blueprint. The hard question is how each cell decides which parts to use.

AlphaGenome 2 explained in one line: it is an AI model that predicts how a change in DNA affects one specific cell type at a time. Not a tissue average. Not a rough guess across the whole body. One cell type, one prediction.

This guide explains what AlphaGenome 2 does, what changed since the first version, what the numbers really mean, and where the honest limits sit.

## Why the same DNA reads differently in every cell

Think of DNA as a recipe book with about three billion letters. Only a small part of it codes for proteins, the building blocks of the body. The rest controls when and where those proteins get made. A single-letter change in that control region can switch a gene on in one cell and leave it off in another.

That is why most DNA variants linked to disease are so hard to read. A variant might matter in a gut cell and do nothing in a blood cell. Test the whole tissue at once and the signal drowns in the average. This is the blind spot AlphaGenome 2 was built for.

It is a real problem, not a theoretical one. Genome studies keep finding variants tied to conditions like diabetes and inflammatory bowel disease, yet the variant often sits far from any gene, in a stretch of DNA that looks like nothing. Researchers then face a slow, expensive hunt to work out what it does, and in which cell.

## What AlphaGenome 2 actually does

Give the model a stretch of DNA up to one million letters long, plus one condition: a tissue, a cell type, or a cell state. It predicts how that DNA behaves in that condition. Which genes switch on. How open the DNA is. How it is marked and packaged.

You can also feed it a condition built from your own single-cell experiment. The model has a cell encoder that turns a small sample, even a few dozen cells, into a condition it can work with. No retraining is needed.

Put simply, AlphaGenome 2 explained for a non-scientist: give it DNA and a cell type, and it tells you what that DNA does in that cell.

### One cell type at a time

The first AlphaGenome, previewed in June 2025 and published in Nature in January 2026, did something different. It read a million letters of DNA and predicted thousands of readouts at once, across bulk tissues and cell lines. That was powerful, but it was always an average. AlphaGenome 2 flips the design. It takes one condition at a time and keeps that condition in mind through every layer of the model. The same DNA can now give a different answer for a gut cell and an immune cell, because the model knows which cell it is talking about.

### Trained on more than 107 million cell profiles

Scale is the other half of the story. DeepMind trained the model on more than 107 million individual cell profiles from human and mouse samples, across 68 tissues. It also learned from bulk data and cell-type-resolved DNA methylation data. That is a lot of examples of how the same DNA gets read differently.

DeepMind also released two precomputed resources for researchers who do not want to run big comparisons themselves. And the original AlphaGenome stays in service. AlphaGenome 2 does not replace it. The new model costs more compute per prediction, so a genome-wide atlas like the one built with the first model is not practical with version two.

## AlphaGenome 2 explained: the numbers, in plain words

DeepMind tested both models on TenK10K, a single-cell map of 28 primary immune cell types. The headline result: AlphaGenome 2 found 30% of the variants scientists believe are causal, while keeping its precision at 90%. The first model, tuned for single-cell data, found 16%.

### What 90% precision and 30% recall mean

What does that mean in plain words? Precision of 90% means that when the model points at a variant and says this one matters, it is right about nine times in ten. Recall of 30% means it finds about three in ten of the variants that are really there. Finding nearly twice as many, at the same precision, is real progress. It is also honest to say that seven in ten are still missed. That number matters, and we will come back to it.

The model also got better at direction. It called whether a variant turns a gene up or down correctly 43% of the time, against 23% for the first model. Direction matters because a drug that pushes a gene the wrong way can do harm.

### The inflammatory bowel disease example

Then there is the example DeepMind shared. Researchers had spent about 18 months of lab work tracing a variant linked to inflammatory bowel disease. The variant, rs2836882, sat in a group of seven candidates. AlphaGenome 2 picked it out and linked it to a gene called ETS2, roughly 290,000 letters away on the DNA strand. It reached the same answer in about 30 minutes. James Lee, the lead researcher on the original study, told Nature that the model figured out in half an hour what took his team a year and a half of experiments.

One example is not proof. It is a strong hint of what faster variant hunting could look like. Independent coverage of the launch reports the same figures, and stresses the same limits.

## Who can use it, and how

AlphaGenome 2 is available now for non-commercial research through the AlphaGenome API. DeepMind also published interactive Colab tutorials for expression conditioning. Commercial research access is in private preview. DeepMind plans to add AlphaGenome 2 Skills to Google Antigravity, its agentic development platform, soon.

Two things to know before you get excited. First, the model weights are not released yet. DeepMind says they will follow final publication. Second, DeepMind runs a community spotlight program for researchers, with extra API quota and credits. Applications close on 30 November.

For now, the practical audience is research labs, not the public. If you work in genomics, the API is the door. If you do not, the value is indirect: faster research, and one day, better drugs.

## What it can’t do yet

This is the section most launch coverage skips. It matters most.

AlphaGenome 2 still misses most of what it looks for. On DeepMind’s own headline test, it found 30% of the likely causal variants at high precision. That leaves about seven in ten unflagged. Doubling recall from 16% to 30% is real. It is not the same as solving the problem.

It is a research tool, full stop. DeepMind states plainly that neither AlphaGenome model is validated or approved for clinical or diagnostic use. Do not expect a doctor to use it to read your genome. That is not what it is for, and it is not ready for that.

The numbers are DeepMind’s own. The technical paper is a preprint, not yet peer reviewed, and there has been no independent replication at launch. The headline test was an immune-cell dataset, so performance in other parts of the body is less established. And the model learns patterns from observational data, not cause and effect. It cannot tell you what happens if you knock a gene out or switch it on. It predicts what the data suggests, nothing more.

None of this makes the model useless. It makes it a tool with a clear job and clear edges. Good tools have both.

## Why it matters even if you never look at a genome

Most of us will never open a genome browser. The payoff here is further down the line, in drug discovery. When researchers can point at the cell where a variant acts, they can aim a drug at the right place instead of guessing. Genomics Ltd, for one, says it plans to build AlphaGenome 2 into its Mystra drug-discovery platform.

There is a bigger pattern too. AlphaGenome 2 joins a run of AI models that read biology the way cells actually work: one cell at a time. Digital twins do something similar for machines and systems, building a virtual copy to test what happens before it happens in the real world. AI agents are starting to run the routine steps of research while humans check the results. Even humanoid robots follow the same idea in hardware, built to work in human spaces one task at a time. The tools are different, but the direction is the same: models and machines that handle the slow, repetitive part of a job so people can spend their time on the questions that need judgment.

That is the real promise. Not a model that replaces scientists, but one that hands them a shorter list of suspects and lets them test the best ones first.

## Frequently asked questions

**What is AlphaGenome 2?**

AlphaGenome 2 explained in a sentence: it is Google DeepMind’s DNA sequence model, announced on 11 November 2026. It predicts how a DNA variant changes gene activity in one specific cell type or cell state, instead of giving a tissue-wide average.

**How is it different from the first AlphaGenome?**

The first model predicted readouts for many tissues at once. AlphaGenome 2 works one cell type or state at a time, and it can adapt to a researcher’s own single-cell data. On one immune-cell test it found nearly twice as many causal variants at the same precision.

**Can AlphaGenome 2 diagnose disease?**

No. DeepMind says both AlphaGenome models are for research use only and have not been validated or approved for clinical or diagnostic purposes.

**Can I try AlphaGenome 2 myself?**

If you are a non-commercial researcher, yes, through the AlphaGenome API, with Colab tutorials to start. Commercial access is in private preview, and the model weights are not public yet.

**Will it replace lab experiments?**

No. It predicts from DNA sequence alone, and it still misses about seven in ten causal variants on its headline test. Lab work remains the check that turns a prediction into a fact.

## The bottom line

AlphaGenome 2 is a focused step forward. It takes the hardest part of reading DNA, the fact that the same code means different things in different cells, and gives researchers a tool built for exactly that problem. The numbers are strong, the example is striking, and the limits are stated in plain sight.

Watch what happens next. Independent replication, wider benchmarks beyond immune cells, and the first drug-discovery results will tell us whether this was a good launch or a real shift. For now, AlphaGenome 2 explained in one line is this: a better map of how DNA is read, cell by cell, with plenty of blank space left to fill.
