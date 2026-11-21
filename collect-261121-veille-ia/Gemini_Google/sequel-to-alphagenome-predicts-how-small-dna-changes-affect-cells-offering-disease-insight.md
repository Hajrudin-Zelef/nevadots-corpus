---
id: collect-261121-veille-ia/gemini-google/sequel-to-alphagenome-predicts-how-small-dna-changes-affect-cells-offering-disease-insight
title: "Sequel to AlphaGenome predicts how small DNA changes affect cells, offering disease insight"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["cell", "data", "alphagenome", "cells", "learning", "model", "disease", "type"]
source: https://www.chemistryworld.com/news/sequel-to-alphagenome-predicts-how-small-dna-changes-affect-cells-offering-disease-insight/4024528.article
source_anchor: ""
source_lines: [1, 37]
section: "Gemini_Google"
---
The latest version of AlphaGenome can predict how small variations in DNA sequences can influence genes in a range of different cells. This artificial intelligence (AI) model from Google DeepMind should allow researchers to better investigate multiple cell types relevant to disease.

The first AlphaGenome worked with DNA sequences up to one million base pairs long and predicted how variations in the sequence changed gene activity. Most of the training data was from bulk tissue, however, which meant measurements represented tissues rather than individual cell types. This blurred the subtle but important differences between closely related cells. To address this, over 100 million single-cell profiles across 68 tissues were part of the training data for AlphaGenome 2.

By zeroing in on individual cell types and cell states, and changing the AI architecture, the new version identified twice as many variants that change how genes are expressed in cells, say its creators. ‘We wanted to improve the cell type specificity, as well as increase the granularity of our training data,’ says Ziga Avsec, a genomics researcher with DeepMind. ‘The new model predicts one cell type at a time and in that way it can use the full bandwidth to focus on that.’

The updated deep-learning model, which was described in a preprint that has not been peer reviewed, takes into account the same one million base pairs of DNA as before, both protein-coding and non-coding regions, but also accounts for the cell type or cell state its predicting for. In a blogpost, its developers describe it as a ‘step-change improvement’ in variant predictive performance.

Artificial intelligence (AI) is an umbrella term often incorrectly used to encompass a variety of connected but simpler processes.

**AI** is the ability of machines and computer programmes to perform tasks that typically only humans could do, such as reasoning, responding to feedback and decision making.

**Generative AI** is a newer variant of AI that analyses and detects patterns in training datasets to generate original text, images and videos in response to requests from users. ChatGPT, Microsoft Copilot, Google Gemini and more recently X’s Grok are all examples of chatbots that use generative AI.

**Neural networks** are an interconnected array of artificial neurons, akin to biological brains, that identify, analyse and learn from statistical patterns in data.

**Machine learning** is a subset of AI that allows machines to learn from datasets and make predictions based on new data, without programmers explicitly asking it to do so. Machine learning models improve their performance as they receive more data.

**Deep learning** is an enhanced type of machine learning that uses neural networks with many layers to analyse complex data from very large datasets. Applications of deep learning include speech recognition, image generation and translation.

**Large language models or LLMs** are a type of deep learning trained on large amounts of data to understand and generate language. LLMs learn patterns in text by predicting the next word in the sequence and these models are now able to write prose, analyse text from the internet and hold dialogues with users.

*Written by the* Chemistry World *team*

‘Any biologist who has generated single-cell RNA sequencing data can plug it into the model and get predictions about the effect of variants given their context,’ says Adam Gayoso at DeepMind. ‘The model generalises well, but we can’t test every possible cell state.’ The advance follows on the heels of the recently unveiled AlphaGenome Atlas, which charted the molecular effects of mutating every DNA base pair in the human genome one by one.

Large genetic studies can highlight a DNA region that is linked to a disease, but experiments must then uncover specific DNA variants that are responsible. To demonstrate the power of AlphaGenome 2 it was tasked with revisiting a study that experimentally tracked down a non-coding DNA sequence that dialled up a gene responsible for stronger inflammation in specific immune cells in inflammatory bowel disease (IBD). In the test, the model correctly pinpointed which DNA variant was responsible, the gene that was affected and the cell type where it mattered most – macrophages, a type of immune cell.

‘We currently experimentally test variants to identify ones that are likely to affect gene expression,’ notes James Lee at the Francis Crick Institute, UK, who led the original IBD discovery. ‘This [new model] would speed up that process, providing the predictions are accurate. This is a big if though, and some degree of experimental validation is still likely to be needed.’

The developers agree. ‘There are still many variants which the model makes a prediction of zero, which usually means that it doesn’t know how the variant acts, so it is not perfect,’ says Avsec. ‘People will still need to do a lot of experimental follow-up to confirm hypotheses generated by the model.’

‘Sequence-to-function models, like AlphaGenome, have existed for a decade now,’ says Peter Koo, a quantitative biologist at Cold Spring Harbor Laboratory, US. ‘Here they’ve essentially tuned the model to be more cell-type aware. A lot of the advance is in the engineering needed to make that approach work well.’

He is also impressed by the scale of the model and excited about its potential applications. ‘Access to the full model will let us explore how it can help us understand the effects of genetic variation across cell types,’ Koo says. ‘It’ll take some time to figure out where it works well and what its limitations are.’

## No comments yet
