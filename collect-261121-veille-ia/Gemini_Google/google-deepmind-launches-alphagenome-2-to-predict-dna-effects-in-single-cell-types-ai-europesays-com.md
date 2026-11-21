---
id: collect-261121-veille-ia/gemini-google/google-deepmind-launches-alphagenome-2-to-predict-dna-effects-in-single-cell-types-ai-europesays-com
title: "Google DeepMind launches AlphaGenome 2 to predict DNA effects in single cell types - AI | EUROPESAYS.COM"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "model", "deepmind", "alphagenome", "access", "types", "first", "says"]
source: https://www.europesays.com/ai/237167/
source_anchor: ""
source_lines: [1, 31]
section: "Gemini_Google"
---
A model that says which cells a DNA change disturbs is more useful to drug hunters than one that only says a change matters somewhere in the body. Whether founders get to build on it depends on an access list that is still mostly closed.

Google DeepMind released AlphaGenome 2 on November 11, an update to its DNA-sequence model that predicts how variants in the genome change gene activity in specific cell types rather than in whole tissues. According to DeepMind’s announcement, the model reads one million DNA letters at a time and was trained on more than 100 million individual cell profiles drawn from 68 tissues, in both humans and mice.

Access is split in two. Researchers working non-commercially can use it today through the AlphaGenome API, and commercial users get a private preview. Access runs through DeepMind’s API, which matters a lot if you’re a startup hoping to run it on your own servers.

The original AlphaGenome, released in 2025, worked mostly at the level of tissues and bulk samples. That’s a problem for disease genetics, because a variant linked to, say, an autoimmune condition may only do damage inside one narrow class of immune cell, and a tissue average washes that signal out. Genome-wide association studies have been finding thousands of such variants for years without being able to say which cell is at fault.

DeepMind’s own benchmark numbers are the strongest argument for the upgrade. On the TenK10K dataset, the company says AlphaGenome 2 picked out causal variants 30% of the time against 16% for the first model, and got the direction of the effect right 43% of the time against 23%. DeepMind also says the model finds nearly twice as many expression-altering variants in specific cells as earlier state-of-the-art tools. These are the company’s figures, not an independent test. We haven’t seen a third-party replication.

Sarah Teichmann of the University of Cambridge, who works with the Human Cell Atlas, said in DeepMind’s post that AlphaGenome 2 adapts directly to new single-cell data without retraining. MLQ’s summary of the release adds that it can work from as few as dozens of cells. For a small lab with a rare cell type and a thin dataset, that’s the claim to test first.

The model is not validated or approved for clinical or diagnostic use.

Who gets to build on it

The access split shows where DeepMind sees the first customers. Academic groups such as Teichmann’s Human Cell Atlas community get the API now, while commercial genomics and drug-discovery partners are being brought in through a private preview. For other startups, that suggests a preview deal goes to a partner with its own data and a product to put the model inside.

The model also has a cost. MLQ reported that predictions need considerably more compute than the original, which makes genome-wide atlases impractical with this version, and that the first AlphaGenome stays in service for broader jobs. So a founder who wants to scan every variant in a patient cohort across hundreds of cell types will pay for that in compute and API quota, if DeepMind lets them in at all. That’s the catch. A community spotlight program that may offer API credits closes on November 30, per the same report.

Frankly, the open question for startups is less about accuracy than about dependence. If your product sits on a closed API owned by a lab that also runs Isomorphic Labs, its drug-discovery sister company, you’re building on a supplier who could one day compete with you. DeepMind hasn’t said anything about how it would handle that conflict, and nothing in the announcement suggests Isomorphic gets different access from anyone else. But a company with its own pipeline and first call on the best models has an obvious head start.

The smarter play for a biotech or AI startup is probably to bring something DeepMind doesn’t have. Proprietary single-cell data from a specific disease, a wet lab that can test predictions, or a clinical dataset the model can be checked against all give you a stronger hand than an API key ever will. The model predicts. Someone still has to run the experiment.

Also read: Helsing launches Clarity, a European rival to Palantir’s Maven • Why a Spoken Sentence Makes a Poor Prompt and How Glaido Cleans It Up Before It Reaches Cursor or Claude Code • Beyond permissions: BotAris’s approach to AI agent authority

The Metropolix, from Startup Fortune

This article is posted in AI News, check it out for more related stories.

Almost there. Sign in and your reply posts straight away.
