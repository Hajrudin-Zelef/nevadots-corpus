---
id: collect-261121-veille-ia/gemini-google/google-deepmind-s-alphagenome-2-improves-dna-variant-detection
title: "Google DeepMind's AlphaGenome 2 Improves DNA Variant Detection"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["alphagenome", "cell", "deepmind", "google", "model", "variant", "gene", "researchers"]
source: https://www.techrepublic.com/article/news-google-deepmind-alphagenome-2-genetic-variants/
source_anchor: ""
source_lines: [1, 45]
section: "Gemini_Google"
---
Every cell in your body carries the same DNA blueprint, yet a gut macrophage reacts to genetic glitches completely differently than a heart muscle cell.

Google DeepMind announced AlphaGenome 2 on Nov. 11, introducing a DNA analysis model that predicts how genetic variations influence individual cell types and states.

Trained on more than 107 million single-cell profiles across 68 tissues, the model nearly doubled its predecessorâs performance on a benchmark for identifying disease-linked causal variants, according to the company.

The improvement could help researchers narrow down genetic drivers of disease and identify promising drug targets before moving into costly laboratory experiments.

The original AlphaGenome, previewed in June 2025 and published in Nature in January 2026, predicted molecular activity across thousands of tissue experiments simultaneously, an approach DeepMind said âspread the modelâs capacity thin.â AlphaGenome 2 replaces that parallel design with a conditional architecture.

It takes up to one million DNA letters alongside one specific biological conditionâa tissue, cell type, or cell stateâand evaluates them together at every stage.

This allows researchers to capture subtle regulatory shifts, including newly added DNA methylation readouts alongside gene expression, histone modifications, and chromatin accessibility.

| Metric/Feature | Original AlphaGenome | AlphaGenome 2 | 
|---|---|---|
| Causal Variants Identified (TenK10K) | 16% | 30% | 
| Directional Gene Effects Predicted | 23% | 43% | 
| Prediction Architecture | Parallel bulk tissue outputs | Single-condition conditional model | 
| Training Scope | Bulk tissue and cell lines | 107M+ single-cell profiles across 68 tissues | 

âWhat makes AlphaGenome 2 exciting is how it adapts directly to new single-cell data without retraining, predicting how DNA variants act in newly profiled cell states from even a small sample of cells,â said Sarah Teichmann, co-founder of the Human Cell Atlas consortium and professor at the University of Cambridge, in Google DeepMindâs announcement.

## Real-world validation and compute reality

In testing, DeepMind demonstrated the modelâs ability to pinpoint disease mechanisms without lab-heavy trial and error:

- **Inflammatory Bowel Disease (IBD):** AlphaGenome 2 computationally reproduced a landmark study showing how a distant regulatory variant overactivates the ETS2 gene specifically inside gut macrophages.
- **COVID-19 Immune Response:** Using unseen mRNA vaccine data, the model separated activated T cells from resting ones, showing that a severe COVID-19 variant disrupts gene activity far more intensely in activated cells.

However, higher cellular resolution demands heavier compute. Genomics lead Å½iga Avsec told The Verge that AlphaGenome 2âs detailed predictions are âmore expensiveâ to generate, meaning it will coexist withârather than replaceâthe original model used for massive, genome-wide scans.

## How AlphaGenome 2 affects researchers and drug developers

For biotech firms, pharmaceutical teams, and academic labs, AlphaGenome 2 acts as a high-speed triage engine before costly wet-lab experiments begin. Because drug discovery relies on targeting exact proteins inside specific cells, pinpointing whether a variant turns a gene up or down narrows down therapeutic candidates faster.

âWhen you develop drugs, youâre targeting specific proteins and specific cell types,â Avsec told The Verge.

Academic researchers can access AlphaGenome 2 for free through the AlphaGenome API, while commercial organizations can apply for a private preview. DeepMind also plans to introduce AlphaGenome 2 Skills for its Google Antigravity platform.

The model remains intended for non-clinical research and is not approved for diagnostic use. Its predictions still require experimental validation before they can inform therapeutic decisions.

For pharmaceutical companies and research laboratories, the immediate opportunity is to narrow down which genetic variants deserve closer investigation. Whether those predictions translate into faster drug development will depend on how well they hold up in laboratory testing.

**Other news: Google is expanding its Gemini Intelligence features beyond its own Pixel lineup, bringing more personalized AI assistance to the Pixel 10 series and Motorola Signature 27 as it pushes to make smartphones more context-aware.**
