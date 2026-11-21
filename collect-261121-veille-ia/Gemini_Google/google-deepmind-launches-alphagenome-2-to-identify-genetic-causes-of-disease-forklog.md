---
id: collect-261121-veille-ia/gemini-google/google-deepmind-launches-alphagenome-2-to-identify-genetic-causes-of-disease-forklog
title: "Google DeepMind launches AlphaGenome 2 to identify genetic causes of disease | ForkLog"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "genetic", "disease", "gene", "cell", "google", "deepmind", "model"]
source: https://forklog.com/en/google-deepmind-launches-alphagenome-2-to-identify-genetic-causes-of-disease/
source_anchor: ""
source_lines: [1, 51]
section: "Gemini_Google"
---
# Google DeepMind launches AlphaGenome 2 to identify genetic causes of disease

Google DeepMind unveils AlphaGenome 2 to predict gene effects of DNA changes.

Google DeepMind unveiled AlphaGenome 2, an AI model that predicts how DNA changes affect gene activity across different cell types. According to the developers, the system finds nearly twice as many significant mutations as the previous version.

The model was trained on more than 107 million single-cell profiles from 68 human and mouse tissues. It analyzes sequences up to one million nucleotides long.

DNA is almost identical throughout the human body, but it functions differently across tissues. The same mutation can disrupt functions in some immune cells while leaving others almost unaffected.

The first version of AlphaGenome assessed the effects of genetic changes mainly at the level of tissues and cell lines. AlphaGenome 2 also factors in their state—for example, during disease or after vaccination.

## AI reproduced part of a study’s findings in 30 minutes

In testing, AlphaGenome 2 reproduced part of the findings of a 2024 study on the causes of inflammatory bowel disease.

Scientists found that a genetic variant in a noncoding region of DNA increases activity of the ETS2 gene in certain intestinal immune cells—macrophages. This promotes inflammation.

AlphaGenome 2 identified the same variant computationally and showed that it affects ETS2 specifically in intestinal macrophages, not in other analyzed cell types.

Nature noted that the original discovery required gene editing and complex molecular biology experiments. James Lee, who led that study at the Francis Crick Institute, said his team needed a year and a half, while the AI reached the same answer in about 30 minutes.

In another test, the model analyzed data from a study of mRNA vaccination against COVID-19. The system determined that one genetic variant associated with severe disease disrupts gene activity more strongly in activated immune T cells than at rest.

## Results improved nearly twofold

According to Google DeepMind, on the TenK10K dataset covering 28 immune cell types, AlphaGenome 2 identified 30% of causal genetic variants.

For the previous version, additionally trained on single-cell data, and other specialized tools, the figure was 16%.

When determining the direction of a mutation’s effect—whether it increases or suppresses gene activity—the gap was the same: 43% versus 23%. Both metrics were obtained on the same dataset.

The developers attributed the progress to a new architecture: the model makes predictions separately for each cell type or state.

Thanks to this, AlphaGenome 2 can handle previously unseen cell states without additional training. Researchers only need to provide gene activity data from a few dozen cells.

The system also predicts DNA methylation and estimates characteristics not measured in an experiment—for example, chromatin accessibility from gene activity data. This could help scientists select the most promising directions for laboratory research.

## Capabilities and limitations

Sara Teichmann, cofounder of the international Human Cell Atlas project, highlighted AlphaGenome 2’s ability to work with new data without retraining. In her view, this is especially important for studying cells whose state changes under disease.

At the same time, the model currently delivers reliable predictions for only a small fraction of mutations. Geneticist and Genomics CEO Peter Donnelly noted that at the confidence thresholds used by the model’s authors, this is about 2% of the 6.8 million analyzed.

However, among variants that genetic studies already classify as likely causal, the model makes such predictions in the relevant cell type about 15 times more often.

According to the expert, the development could help identify targets for new drugs. Its effectiveness still needs to be systematically evaluated across many diseases.

Another limitation is computational cost. The developers told The Verge that more detailed analysis is more expensive than in the first AlphaGenome. As a result, the new system is not yet suitable for genome-wide calculations on the same scale as its predecessor.

In October, Hugging Face unveiled the open AI model Carbon-X, which analyzes genetic sequences and answers questions about them in natural language.
