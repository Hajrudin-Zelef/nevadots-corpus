---
id: collect-261121-veille-ia/gemini-google/google-deepminds-alphagenome-2-predicts-dna-variant-effects-in-30-minutes
title: "Google DeepMind’s AlphaGenome 2 predicts DNA variant effects in 30 minutes"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "cell", "variants", "types", "gene", "minutes", "deepmind", "effects"]
source: https://dongascience.com/en/news/80848
source_anchor: ""
source_lines: [1, 53]
section: "Gemini_Google"
---
An updated version of an artificial intelligence (AI) model that predicts how deoxyribonucleic acid (DNA) variants affect gene activity in different cell types has been released. It is expected to help identify disease-causing variants much faster, as it was able to reproduce in 30 minutes part of the results that previously required a year and a half of experiments.


Google DeepMind announced on its website on the 11th (local time) that it has released AlphaGenome 2, an AI model that predicts how noncoding DNA sequences influence gene expression and related processes. As an upgraded version of last year’s AlphaGenome 1, it can predict the impact of genetic variants across hundreds of human and mouse cell types.


DNA contains regions that encode information for making proteins and regions that do not. The latter are called “noncoding DNA” and account for about 98% of the human genome.


Noncoding DNA was once thought to have no particular function, but as it became clear that some of it regulates gene expression, understanding its role has become important—yet remains challenging. Regulatory functions often appear predominantly in specific cell types.


Ziga Avsec, a DeepMind researcher leading the AlphaGenome project, said in an interview with the international journal Nature, “To understand variants associated with genetic diseases, we need to know which cell types are affected.”


In September, DeepMind released the AlphaGenome Atlas, which aggregates impact predictions for every possible single–base change in human DNA. Using AlphaGenome 1, the team predicted the effects of about 9 billion variants. However, the model had limitations in determining exactly which cell types—such as B cells, a type of immune cell, or pancreatic beta cells—the variants acted in.


To account for the fact that genetic variants have different effects in different cell types, the DeepMind team expanded its training data. Unlike AlphaGenome 1, which was trained mainly on data from samples containing mixtures of various cell types, AlphaGenome 2 was also trained on single-cell data.


The single-cell data set included information on gene activity and related features measured in more than 100 million individual cells from 68 tissues. The training covered 637 human cell types and 311 mouse cell types.


The prediction approach has also changed. AlphaGenome 1 takes a DNA sequence as input and outputs predictions in parallel for a predefined set of tissues and cell lines. AlphaGenome 2, by contrast, takes both the DNA sequence and information about the specific cell to be analyzed as input. Adam Gayoso, a DeepMind researcher, explained, “This allows us to capture differences between cell types much more precisely.”

AlphaGenome 2 demonstrated its performance by rapidly reproducing part of an existing study. In 2024, a team led by James Lee, a group leader at the Francis Crick Institute in the UK, reported that DNA variants associated with inflammatory bowel disease (IBD) cause excessive activation of the gene ETS2 in immune cells. Because the variants are located in a “gene desert” far from protein-coding genes, it had been difficult to pinpoint how they affect cells.


Lee’s team used molecular biology experiments, including genome editing, to verify the effect of the variants and propose new therapeutic targets. The DeepMind team used AlphaGenome 2 to predict, in just 30 minutes, that these variants selectively increase ETS2 gene activity in macrophages, a type of intestinal immune cell.


Lee said, “The fact that AlphaGenome 2 found in 30 minutes the answer that took us a year and a half of experiments is, from our perspective, a reassuring result.”


The scope of AlphaGenome 2 is not limited to the cell types used in its training. If researchers provide new single-cell gene activity data from, for example, diseased cells, the model can also predict the effects of genetic variants in those cells.


Peter Kharchenko, an assistant professor at Cold Spring Harbor Laboratory in the United States, commented, “AlphaGenome 2 is part of a recent trend in AI for interpreting noncoding DNA by incorporating information on cell states.” He added, “The idea itself is not entirely new, but the engineering achievement in implementing it at this scale and the resulting performance are quite impressive.”


Researchers can use AlphaGenome 2 free of charge for non-commercial purposes. For commercial research, it is available as a closed early-access offering.


AI predictions will not immediately replace experiments. Lee said his team plans to test AlphaGenome 2’s predictions on other noncoding variants they have characterized in specific cell types.


If the predictions prove accurate, they could be used to prioritize which variants warrant detailed follow-up experiments, thereby speeding up research. “I don’t think AlphaGenome 2 eliminates the need for experiments,” he said, “but it is possible that such a day will come with time.”


<Reference>

deepmind.google/blog/alphagenome-2-deciphering-how-cells-read-dna-to-better-understand-disease/
