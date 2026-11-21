---
id: collect-261121-veille-ia/gemini-google/google-deepminds-alphagenome-takes-aim-at-one-of-genetics-biggest-problems-bigdatawire
title: "Google DeepMind’s AlphaGenome Takes Aim at One of Genetics’ Biggest Problems - BigDATAwire"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-09"]
keywords: ["alphagenome", "atlas", "researchers", "variants", "variant", "changes", "model", "deepmind"]
source: https://www.hpcwire.com/bigdatawire/2026/09/09/google-deepminds-alphagenome-takes-aim-at-one-of-genetics-biggest-problems/
source_anchor: ""
source_lines: [1, 29]
section: "Gemini_Google"
---
What does a pile of sand have in common with a folding protein? Not much....

## Off the Wire Press Releases

Google DeepMind has released a huge new dataset that could make it easier for scientists to study how changes in our DNA affect the way the body works. Called AlphaGenome Atlas, it contains predictions for more than 9 billion possible single-letter changes in the human genome.

The dataset is about one petabyte in size and was created using DeepMind’s AlphaGenome AI model. Instead of researchers having to run the model themselves each time they want to study a genetic variant, DeepMind has already done much of that work. Scientists can now search the Atlas for variants they are interested in and see what effects the model predicts.

Nine billion sounds like an enormous number, but it comes down to some fairly simple math. The human genome contains roughly 3 billion DNA letters. There are four possible letters: A, C, G and T. The letter at any given position could be changed to one of the other three. That leaves roughly 9 billion possible single-letter changes.

Scientists are interested in these variants because some can affect how our genes work and contribute to disease. The hard part is separating those important changes from the many that have little effect, particularly across the 98% of the genome that does not directly code for proteins.

AlphaGenome provides the AI behind all of this. The model predicts how genetic changes could affect molecular processes such as gene expression, RNA splicing and chromatin accessibility. AlphaGenome Atlas takes those predictions and precomputes them across the genome, rather than requiring researchers to run the model one variant at a time.

The result is far more than a database with one answer for each of the 9 billion possible changes. DeepMind says the Atlas contains thousands of molecular-effect predictions for every variant, spanning hundreds of human and mouse cell types and tissues. At that scale, generating the data is only part of the challenge. Researchers also need a practical way to sort through it and identify the variants that stand out.

To help researchers sort through all of those predictions, DeepMind has also introduced the AlphaGenome Variant Impact (AVI) score. It combines predictions from AlphaGenome and AlphaMissense, DeepMind’s model for predicting the impact of protein-altering DNA variants, into a single number.

Researchers can use the score to rank variants based on their potential impact. Importantly, it works across both the 2% of the genome that codes for proteins and the remaining 98% that does not. The Atlas also shows which molecular processes, such as RNA splicing or gene expression, are predicted to be affected by a particular variant.

Researchers are already putting the Atlas to use. In its blog post announcing the new resource, the DeepMind team highlighted work using the system on data from the UK Biobank:

“To test how AlphaGenome Atlas can improve our ability to find non-coding variants affecting human traits, Gareth Hawkes, a Medical Research Council fellow at the University of Exeter, applied AlphaGenome Atlas to whole-genome data from over 54,000 UK Biobank participants, which made these elusive signals more obvious. By grouping rare variants based on their predicted molecular effects, Hawkes uncovered 22% more non-coding genetic associations, which would otherwise have not been detectable in the statistical noise.”

The Atlas is also being tested in rare disease research. Working with the GREGoR Consortium, researchers from the Broad Institute used the AVI score to look for potentially important variants that had been missed in previous research. They identified a variant affecting DNM1, a gene strongly associated with epileptic encephalopathy. AlphaGenome predicted that the variant created an incorrect RNA splice site, and subsequent experiments validated that prediction.

For now, the Atlas is free for researchers to use through the website and AlphaGenome API. DeepMind says commercial access through Google Cloud is coming soon.

The company also says this is really just a starting point. The Atlas will change as AlphaGenome and its other AI models get better. And while it won’t tell researchers which genetic changes cause disease, it could help them figure out which ones are worth spending more time on and testing in the lab.
