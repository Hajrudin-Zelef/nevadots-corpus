---
id: collect-261121-veille-ia/gemini-google/google-deepmind-mapped-all-disease-before-it-happens-cystems
title: "Google DeepMind Mapped All Disease Before It Happens. | CYSTEMS"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-08"]
keywords: ["variant", "google", "deepmind", "disease", "mapped", "before", "happens", "same"]
source: https://cystems.io/blog/ben-lehner-this-isnt-an-alphafold-moment
source_anchor: ""
source_lines: [1, 37]
section: "Gemini_Google"
---
# Google DeepMind Mapped All Disease Before It Happens.

Nine billion mutations, precomputed and handed to science for free on September 8. Not one of them is cleared to diagnose anybody. The scientists closest to the work, Ben Lehner at the Wellcome Sanger Institute among them, are impressed and unconvinced at the same time, and the gap between those two reactions is the story.

On September 8, Google DeepMind released AlphaGenome Atlas: a precomputed prediction for every one of the roughly nine billion single-letter changes that can occur in human DNA. The dataset runs to about a petabyte, more than thirty times the size of the AlphaFold database. Academic access is free now. Commercial access goes to Google Cloud later.

The biology is simple enough to state plainly. Genes are stretches of DNA spelled in four letters. Swap one letter for another and you have a single nucleotide variant. That one swap can change how the gene is read, how the resulting RNA is spliced, and what shape the finished protein folds into. Shape determines function. A protein with the wrong shape usually does the wrong job, and that is what a genetic disease is.

## What the Atlas actually is

Reading DNA stopped being the hard part years ago. The hard part is knowing which differences matter. Sequence a patient with an undiagnosed rare disease and you get thousands of candidate variants, nearly all of them harmless. The clinical question is not what sits in the genome. It is which line to look at first.

Every variant in the Atlas carries an AVI score, the AlphaGenome Variant Impact, one number that merges AlphaGenome's read on regulation and splicing with AlphaMissense's read on protein-altering changes. It covers coding and non-coding regions alike. Google DeepMind puts the scale like this: a score of ten places a variant in the most impactful tenth of the genome, and a score of thirty places it in the top one in a thousand.

The benchmark is the honest way to judge it. On rare-disease cases, as reported by Nature, the causal variant lands inside the top fifty candidates 29.5 percent of the time, against 12.5 percent for CADD, the tool that has been the working standard for a decade. That is a doubling. It is not a solution. Seven times out of ten the answer is still somewhere below the fold.

## The case that made it land

The result that carried the release came out of the Broad Institute. Working with the GREGoR Consortium, Laura Covill and Anne O'Donnell-Luria re-ranked cases that earlier analysis had already been through and closed. The AVI score pushed a variant in DNM1 to the front, a gene tightly linked to epileptic encephalopathy. The variant sits deep inside an intron, a stretch that does not code for protein, which is exactly why previous passes walked past it.

What matters is what the model said next. It did not flag the variant as merely suspicious. It predicted the mechanism: the change creates a splice site that should not be there, and the protein comes out abnormally extended. That is a specific, falsifiable claim, and it is cheap to check. They checked it in the lab. It held, and nearby variants did the same thing.

## What the people closest to it say

The researchers who do this work are impressed and unconvinced at the same time, and both are correct. Ben Lehner at the Wellcome Sanger Institute put it flatly: this is not an AlphaFold moment, and genomic models like this one should not yet be used alone to make clinical decisions. Martin Kircher made the same point from the other side, that a prediction does not replace an experiment or the judgment applied to an individual case. Google DeepMind says it in writing on the release itself: AlphaGenome has not been validated for, and is not approved for, any clinical use.

The limits are structural, not a matter of another training run. The model reads roughly a million base pairs around a variant, and enhancers can sit further away than that and still control the gene. Cell-type-specific regulation remains weak. And most disease is not one letter. It is many variants, a lifetime, and an environment.

## The part worth stealing

The move here is architectural. Nobody can afford to query nine billion variants on demand, so the expensive question was answered once, in advance, and the answers were published as a table. A lab that would have spent a week narrowing candidates now performs a lookup. We build our own Cystems on that principle: find where the costly question repeats, answer it ahead of time, and let the surface serve the answer instantly.

So no, this is not the end of genetic disease, and the people closest to it are the ones saying so loudest. It is a shortlist twice as good as the shortlist we had. For a family waiting years for a diagnosis that never arrives, a better shortlist is worth considerably more than a slogan.

Sources: AlphaGenome Atlas and its technical paper, Google DeepMind, September 8, 2026. Expert commentary from Ben Lehner, Wellcome Sanger Institute, and Martin Kircher, via the Science Media Centre and Nature. The DNM1 case from Laura Covill and Anne O'Donnell-Luria at the Broad Institute with the GREGoR Consortium. Benchmark figures reported by Nature and Scientific American.

I share ideas, lessons, andpractical insights from my work.
