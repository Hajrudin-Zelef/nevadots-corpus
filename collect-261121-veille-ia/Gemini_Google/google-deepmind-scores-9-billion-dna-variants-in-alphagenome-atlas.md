---
id: collect-261121-veille-ia/gemini-google/google-deepmind-scores-9-billion-dna-variants-in-alphagenome-atlas
title: "Google DeepMind scores 9 billion DNA variants in AlphaGenome Atlas"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "google", "atlas", "variants", "deepmind", "avsec", "billion", "variant"]
source: https://pivotnews.ai/healthcare/google-deepmind-scores-9-billion-dna-variants-in
source_anchor: ""
source_lines: [1, 15]
section: "Gemini_Google"
---
# Google DeepMind scores 9 billion DNA variants in AlphaGenome Atlas

Researchers at Exeter and the Broad Institute have already used the dataset's single impact score to shrink variant shortlists, and Google DeepMind says it has since launched AlphaGenome 2 to predict effects inside individual cell types.

Google DeepMind has run its AlphaGenome model across every possible single-letter change in human DNA, all 9 billion of them, and published the results as a dataset called AlphaGenome Atlas. Research scientist Žiga Avsec, who leads the company's genomics team, described the work in a November 17 post on blog.google as part of Google's “Ask a Scientist” series. AlphaGenome itself launched in June 2025 and takes a partial DNA sequence of up to 1 million base pairs, producing thousands of predictions about what that sequence does; the Atlas followed in September 2026.[1]

The mechanism is a triage problem. Sequencing a patient with a rare disease can turn up roughly 4 million variants against the reference human genome, and filtering for rare ones still leaves hundreds or thousands of candidates, Avsec said. AlphaGenome Atlas returns a single number for each variant, the AVI (AlphaGenome Variant Impact) score, indicating its potential effect across the genome so researchers can rank which changes are worth expensive wet-lab follow-up. Much of the difficulty, Avsec said, sits in non-coding “dark matter” DNA that regulates gene expression and disease risk and whose semantics remain poorly understood.[1]

## Evidence

Avsec pointed to two early users. Gareth Hawkes, a Medical Research Council fellow at the University of Exeter, used the Atlas to cut a list of 1 billion genetic variants down to the most impactful 1 percent and identified 19 variant groups linked to body mass index for further work. Laura Covill, a research fellow at the Broad Institute, used the AVI score while investigating unsolved rare diseases; the Atlas flagged a variant in a gene linked to neurodevelopmental disorders, which Avsec said supplied supporting evidence that helped solve the case.[1]

## What's next

Google DeepMind has also launched AlphaGenome 2, which Avsec said addresses a known limit of the first model: it was trained largely on whole tissues or cell lines, so its predictions average across cell types that behave differently and fail differently in disease. The new version predicts how variants disrupt individual cell types. The team is separately working on confidence measures so researchers know how far to trust each prediction. Early analyses show high accuracy for variants with strongly predicted effects, Avsec said, adding that anyone using AlphaGenome should interpret its output alongside other sources of information. The longer-term goal he described is a full digital simulation of a human cell.[1]
