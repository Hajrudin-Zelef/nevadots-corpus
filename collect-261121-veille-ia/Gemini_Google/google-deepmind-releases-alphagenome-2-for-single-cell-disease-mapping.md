---
id: collect-261121-veille-ia/gemini-google/google-deepmind-releases-alphagenome-2-for-single-cell-disease-mapping
title: "Google DeepMind Releases AlphaGenome 2 for Single-Cell Disease Mapping"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["alphagenome", "disease", "deepmind", "single-cell", "human", "tissue", "across", "google"]
source: https://gfunfacts.online/article/google-deepmind-alphagenome-2-single-cell-mapping
source_anchor: ""
source_lines: [1, 287]
section: "Gemini_Google"
---
On the morning of November 11, 2026, geneticist James Lee watched an algorithmic readout confirm what had previously cost his team eighteen months of wet-lab labor, thousands of liters of reagents, and hundreds of cellular assays.

At the Francis Crick Institute in central London, Lee had spent years chasing a genetic ghost. Genome-wide association studies had repeatedly linked a dusty, barren stretch of chromosome 21 to inflammatory bowel disease (IBD). The stretch was a classic "gene desert"—a 290,000-base-pair expanse devoid of protein-coding instructions. Conventional genetics offered no explanation for why mutations inside this expanse caused severe gastrointestinal inflammation. Only after dissecting rare gut macrophages from human tissue biopsies did Lee's laboratory discover the culprit: a single non-coding DNA switch that folded across a third of a million base pairs to overactivate an inflammatory master-switch gene named *ETS2*.

Less than a mile south, across the Euston Road at Google DeepMind’s headquarters, a computing cluster running the newly completed AlphaGenome 2 reached the same conclusion.

Given only the raw, million-letter sequence of human DNA and the specific molecular profile of an intestinal macrophage, the model evaluated seven ambiguous DNA candidates in the locus, pinpointed the exact causal variant, and mapped its regulatory loop across 290,000 bases directly to *ETS2*. It predicted that the mutation would amplify *ETS2* specifically in those gut macrophages while leaving other immune cells and neighboring tissues largely unperturbed.

The algorithm needed roughly thirty minutes.

"They managed to figure out the answer in thirty minutes, and it took us a year and a half of experiments," Lee recounted following the model's public debut.

The announcement of AlphaGenome 2 on Wednesday, led by DeepMind genomics lead Žiga Avsec, research scientist Adam Gayoso, Natasha Latysheva, and head of AI for Science Pushmeet Kohli, represents a fundamental repositioning in computational biology. For decades, artificial intelligence in genomics operated like a satellite taking long-exposure photographs of an entire city: useful for mapping the highways of bulk tissue, but blind to the individual rooms where cellular pathology actually begins.

AlphaGenome 2 discards bulk averages to inspect biology at single-cell resolution. Trained on more than 107 million individual cell profiles across 68 human and mouse tissues, the neural network predicts not merely what a sequence of DNA does, but how that sequence operates within an isolated cell type or distinct cellular state.

Yet an investigation into DeepMind’s architecture, its reported benchmarks, and the real-world operational bottlenecks of modern drug discovery reveals that this leap into single-cell biology carries structural tensions. Beneath the headline metric—a near-doubling of causal variant recall over its predecessor—lies an unresolved 70 percent blind spot, a mounting compute penalty that makes universal genome atlases impractical, and an escalating power struggle between open academic science and proprietary cloud platforms.

### The "Tissue Average" Delusion

To understand why DeepMind overhauled its genomic engine within sixteen months of releasing its predecessor, one must look at the smoothie problem.

For more than two decades following the sequencing of the human genome, high-throughput biology relied on bulk tissue sequencing. If researchers wanted to understand heart disease, they ground up a piece of cardiac muscle and fed it into a sequencer. The resulting data represented an average of cardiomyocytes, fibroblasts, endothelial cells, vascular smooth muscle cells, and infiltrating immune cells.

"Bulk sequencing is like putting an apple, an orange, and a lemon into a blender and trying to deduce the exact molecular properties of an orange seed by drinking the smoothie," explains an independent computational biologist familiar with the Sanger Institute's comparative mapping datasets. "If an orange seed possesses a fatal defect, its biochemical signal is washed away by the gallons of apple and lemon juice surrounding it."

When DeepMind released the original AlphaGenome in mid-2025—later detailed in *Nature* in early 2026—the model inherited this structural compromise. That first iteration was an engineering monolith: it accepted an input sequence of one million DNA letters and outputted thousands of parallel predictions across hundreds of bulk tissue types and cell lines. It forecasted gene expression levels, chromatin accessibility, histone modifications, and three-dimensional contact maps. To democratize the tool, DeepMind precomputed predictions for all nine billion possible single-letter changes across the human genome, publishing the 1-petabyte AlphaGenome Atlas.

Biologists quickly encountered the boundaries of that approach.

"The original model’s limited cell-type specificity constrained its usefulness for mechanistic discovery," Lee noted. Complex diseases do not unfold uniformly across an organ; they originate in distinct sub-populations caught in fleeting functional states. In systemic lupus erythematosus, a mutation might only manifest when an autoreactive B cell interacts with a follicular helper T cell. In neurodegenerative diseases like Alzheimer's, disease-driving mutations rarely alter neurons directly; instead, they disrupt regulatory switches in microglial cells responding to inflammatory insults.

When AlphaGenome 1 attempted to predict the impact of these alterations, its parallel architecture forced it to spread its representational capacity across hundreds of incompatible tissue signals. The model frequently averaged out the very regulatory spikes that define disease states. Worse, outside of laboratory cell lines, it struggled to distinguish between subtle lineages—such as an unactivated, resting CD4+ T cell and one activated by a viral vaccine.

DeepMind's internal post-mortems recognized that simply adding more single-cell data to the original multi-track framework would fail. The neural network required a clean-sheet redesign: a system that could dynamically shift its internal representation based on cellular context.

The result of that shift is what DeepMind now positions as **AlphaGenome 2 disease mapping**, a framework designed to replace static reference predictions with context-dependent biological simulations.

### Inside the Machine: The Architecture of Conditionality

DeepMind’s technical paper reveals an architectural transformation. Instead of predicting thousands of biological experiments simultaneously across diverse tissues, AlphaGenome 2 is an explicitly conditional sequence-to-function model. It evaluates one million base pairs of DNA, but evaluates them for exactly one biological condition at a time.

```
AlphaGenome 1: Parallel Fixed Outputs
[ 1 Mb DNA Sequence ] ─────────► [ Neural Backbone ] ───┬─► Bulk Liver Track
                                                        ├─► Bulk Brain Track
                                                        ├─► K562 Cell Line
                                                        └─► 1,000s of Parallel Tracks
                                                            (Averaged Signals)
AlphaGenome 2: Dynamic Conditional Architecture
[ 1 Mb DNA Sequence ] ──┐
                        ▼
            [ Convolutional Encoder ] ◄── FiLM Conditioning (256-d Vector)
                        │
                        ▼
            [ 11-Block Transformer ]  ◄── FiLM Conditioning (256-d Vector)
                        │
                        ▼
             [ Expression Decoder ]   ◄── FiLM Conditioning (256-d Vector)
                        │
                        ▼
     Single Specific State Output: (e.g., Activated Intestinal Macrophage)
     - RNA Coverage (CAGE, PRO-cap)
     - Splicing Junctions & Splice Sites
     - Chromatin Accessibility (DNase, ATAC-seq)
     - Histone Marks & TF Binding
     - Chromatin Contact Maps (Hi-C)
     - DNA Methylation (Newly added)
```
The underlying network retains a U-Net-style framework featuring a convolutional encoder, an 11-block transformer backbone, and an expression decoder. However, the conditioning mechanism fundamentally alters how genetic tokens move through this pipeline.

The biological condition—whether an activated microglial cell, a resting CD8+ memory T cell, or a primary brain astrocyte—is converted into a 256-dimensional embedding vector. This vector is not simply appended to the input DNA sequence or tacked onto the final output layer. Instead, DeepMind utilized Feature-wise Linear Modulation (FiLM) layers embedded directly into the convolutional encoder, each of the eleven transformer blocks, and the final decoder.

The engineering rationale behind this decision proved decisive during development. FiLM layers work by applying an affine transformation—scaling and shifting—to the intermediate feature activations of the neural network based on the conditioning input:

$$\text{FiLM}(x; \gamma, \beta) = \gamma(c) \odot x + \beta(c)$$

Where $x$ represents the intermediate genomic representations, and $\gamma(c)$ and $\beta(c)$ are learned functions of the 256-dimensional biological condition $c$.

DeepMind's ablation studies uncovered an essential principle: when the engineering team applied conditionality only to the final decoder or output heads, the model experienced negligible gains over the baseline. The sequence representations extracted by the transformer remained generic and anchored to bulk patterns. Only when the 256-dimensional cell state was fed into the convolutional encoder and the core transformer blocks did the system demonstrate state-specific sensitivity.

By modulating early- and mid-stage representations, the model evaluates the identical DNA letter differently depending on the cellular environment. A transcription factor binding motif that sits dormant in a skin fibroblast is actively processed as an open, high-affinity regulatory anchor if the condition vector signals an immune cell under inflammatory stress.

Alongside gene expression (measured via RNA-seq, CAGE, and PRO-cap), splicing patterns, chromatin accessibility, histone marks, and 3D contact maps, DeepMind integrated DNA methylation predictions. Methylation—the addition of methyl groups to cytosine bases—acts as the epigenetic lock of cellular identity, silencing transposons and switching off lineage-inappropriate genes. By adding methylation to its multi-modal loss function, the network can model whether a specific stretch of regulatory sequence is physically unlocked or epigenetically sealed in a given cell type.

### The Four-Stage Training Pipeline

Building a model capable of bridging single-cell biology with nucleotide-level genetics required a four-stage training pipeline designed to manage severe data fragmentation.

Single-cell sequencing data is notoriously noisy. Unlike bulk assays, which pool millions of cells to yield smooth, consistent signal tracks, single-cell RNA sequencing (scRNA-seq) and single-cell ATAC sequencing (scATAC-seq) suffer from massive "dropout" events—technical artifacts where a expressed gene fails to be captured by the microfluidic droplet and appears as a false zero.

DeepMind bypassed this hurdle by staging the model’s exposure to biological reality:

1. **Massive Multitask Pretraining** : The foundational model was trained across thousands of bulk sequencing tracks alongside 3,977 single-cell RNA and ATAC pseudobulk tracks aggregated from 36 distinct single-cell studies. Pseudobulking—computationally aggregating the reads of identical cell types within a single-cell dataset—allowed the model to absorb clean, lineage-specific signals across 107 million individual cells without choking on single-cell dropouts.
2. **Condition Refinement** : With the weights of the primary genomic encoder largely established, DeepMind froze the bulk of the convolutional and transformer backbones, focusing optimization on the 256-dimensional condition embeddings and the affine FiLM layers. This forced the model to map the precise relationships between distinct cell types, teaching it how gene expression diverges as a hematopoietic blood stem cell gradually differentiates into a mature red blood cell or an immune lineage.
3. **Teacher-Student Distillation on Mutated Genomes** : Predicting variant effects is computationally harder than predicting wild-type sequence function. To sharpen the model's sensitivity to single-letter mutations, DeepMind orchestrated a distillation phase. Eight large "teacher" models were trained on perturbed, synthetic, and mutated genomic sequences. Their collective predictive outputs were then distilled into a single, highly compressed student network, which learned to prioritize subtle shifts in local regulatory syntax.
4. **Post-Training of the Zero-Shot Cell Encoder** : In the final stage, researchers built an external entry point for the broader scientific community. DeepMind trained a lightweight neural encoder to map raw, user-provided single-cell expression profiles directly into the frozen 256-dimensional condition space of AlphaGenome 2.

This fourth stage resolved one of the most stubborn limitations in biological machine learning: how to query a model about a cell state that was not present in its original training registry.

### The Zero-Shot Cell Encoder

During early developer previews, the primary concern among molecular biologists was rigidity. The human body contains hundreds of recognized cell types, but in clinical settings, diseases create pathological cell states that defy standard classification. A tumor-infiltrating exhausted lymphocyte is not identical to a normal circulating T cell; a fibroblast in an idiopathic pulmonary fibrosis scar behaves differently than a healthy dermal fibroblast.

DeepMind’s answer was the zero-shot cell encoder.

Instead of forcing users to retrain or fine-tune AlphaGenome 2 on custom datasets—a process requiring high-performance GPU clusters—the system allows a researcher to pass in raw scRNA-seq expression counts from as few as thirty to fifty cells. The cell encoder condenses the gene expression pattern of those isolated cells, extracts the essential transcriptional markers, and projects the biological condition directly into the model’s 256-dimensional FiLM vector.

```
User's Laboratory Sample (e.g., 50 Biopsy Cells)
                   │
                   ▼
       [ Raw scRNA-seq Counts ]
                   │
                   ▼
        [ Zero-Shot Cell Encoder ]
                   │
                   ▼
     [ 256-d Condition Vector ] ───┐
                                   ▼
 [ 1 Mb Patient DNA ] ───► [ AlphaGenome 2 ] ───► Predicted Variant Impact in
                                                  Patient's Specific Cell State
```
"What makes AlphaGenome 2 exciting is how it adapts directly to new single-cell data without retraining, predicting how DNA variants act in newly profiled cell states from even a small sample of cells," said Sarah Teichmann, a professor of stem cell medicine at the University of Cambridge and co-founder of the global Human Cell Atlas consortium.

To validate this zero-shot generalization, DeepMind's team tested the network against an external dataset it had never encountered: single-cell peripheral blood samples taken from patients during an mRNA COVID-19 vaccine trial.

The model was fed raw cellular counts from both resting memory T cells and T cells that had been activated by the vaccine antigen. AlphaGenome 2’s encoder accurately distinguished between the two states.

Next, the team queried the system with a non-coding genetic variant previously implicated in severe COVID-19 outcomes. In the resting cell condition, AlphaGenome 2 predicted negligible molecular disruption. But when conditioned on the vaccine-activated T cell state, the model predicted that the mutation disrupted an enhancer loop regulating *CXCR6*—a chemokine receptor gene essential for homing immune cells to the lung epithelium. 

The regulatory lesion only existed when the cell was actively defending against an infection. In a traditional bulk model or a resting cell assay, the mutation's mechanism would have remained completely undetectable.

### Deconstructing the Benchmarks: The TenK10K Reality Check

The centerpiece of DeepMind's announcement is its performance on the TenK10K benchmark—a gold-standard single-cell catalog mapping expression quantitative trait loci (sc-eQTLs) across 28 primary immune cell types.

An eQTL is a genetic variant that correlates with changes in gene expression. A *causal* eQTL is the specific mechanical trigger—the exact nucleotide switch driving the expression change, rather than neighboring variants that are merely hitchhiking along due to genetic linkage. For decades, distinguishing the true causal variant from dozens of co-inherited mutations in a single locus has formed a major bottleneck in genetic medicine.

On the TenK10K evaluation, DeepMind reported that AlphaGenome 2 identified causal variants with a recall rate of 30 percent at 90 percent precision. The original AlphaGenome, even after being explicitly fine-tuned on single-cell data, managed only 16 percent recall under identical parameters. When tasked with calling the *direction* of the effect—predicting whether a mutation upregulates or silences a specific target gene—AlphaGenome 2 succeeded in 43 percent of cases, compared to 23 percent for the older system.

| Evaluation Metric (TenK10K Dataset) | Original AlphaGenome (Single-Cell Fine-Tuned) | AlphaGenome 2 (Conditional Sequence Model) | Relative Performance Jump | 
|---|---|---|---|
| **Causal Variant Recall** (at 90% Precision) | 16% | **30%** | +87.5% (1.9x) | 
| **Directional Gene Effect Accuracy** | 23% | **43%** | +86.9% (1.9x) | 
| **Input Modality Scope** | Fixed Parallel Tracks | **Conditional per State (256-d)** | Dynamic Conditioning | 
| **Epigenetic Readouts** | Expression, ATAC, Histone, Hi-C | **Adds DNA Methylation** | +Methylation Tracking | 
| **Adaptability to Unseen Data** | Requires Model Fine-Tuning | **Zero-Shot Cell Encoder** | Retraining Eliminated | 

To the algorithmic community, an 88 percent relative gain over the previous state-of-the-art model is substantial. But an investigative examination of the denominator tells a more sober story: seventy percent of causal genetic variants in primary immune cells still elude the algorithm.

"We have to view these figures through the reality of clinical translation," says Professor Sir Peter Donnelly, CEO of Genomics Limited and former director of the Wellcome Centre for Human Genetics at Oxford. "For those of us using human genetics to find drug targets, the key question at any disease-associated locus is what the variants there actually do: which gene they act on, in which direction, and in which cell types. AlphaGenome 2 will certainly help untangle what is going on at some loci. Its predictions of whether a variant turns a gene up or down, and in which cell types, are markedly better than those of its predecessor. Even with these improvements, AlphaGenome 2 makes confident predictions for only a small subset of the variants that can be queried."

Donnelly’s critique cuts directly to the practical core of pharmaceutical pipeline design.

"What remains an open question is how much it adds to the other evidence we already use to identify the gene behind a GWAS signal. That evidence includes proximity, colocalization with expression and protein QTLs, and rare-variant information. Nobody yet knows the answer."

The limits become more apparent when evaluating the distance over which these variants act. While AlphaGenome 2 can accept a context window of up to one million DNA bases, its predictive confidence declines rapidly as regulatory switches move further away from their target gene promoters. Enhancers located more than 100,000 base pairs away—such as the 290,000-base-pair loop in the *ETS2* case—remain difficult exceptions rather than standard outputs. 

Furthermore, DeepMind’s numbers originate entirely from its own technical preprint, which has not yet undergone independent peer review. Skepticism is informed by past experience: after DeepMind released the original AlphaGenome, an independent validation study led by researchers at the Charité University Hospital in Berlin identified noticeable drop-offs in expression prediction accuracy when the system was evaluated on primary tissue samples outside the canonical ENCODE and Roadmap consortia frameworks.

Whether AlphaGenome 2 maintains its 30 percent recall across complex non-immune tissues—such as discrete subtypes of the human central nervous system or dense tumor microenvironments—remains an open scientific question.

### The Retrospective Trap: Lessons from the *ETS2* Discovery

DeepMind’s reliance on the *ETS2* case study illustrates both the power and the methodological limitations of modern biological AI.

The scientific narrative presented by DeepMind is undeniable: James Lee’s lab spent eighteen months proving that a non-coding variant at chromosome 21q22.11 triggers Crohn’s disease and ulcerative colitis by dysregulating *ETS2* via a macrophage-specific enhancer loop. AlphaGenome 2 replicated the result in thirty minutes.

```
Chromosome 21q22.11 Non-Coding Locus:
[ Risk Variant Switch ] ─── (290,000 bp Distance) ───► [ ETS2 Gene Promoter ]
         │                                                      │
         ▼                                                      ▼
  Active only in                                       Excess Inflammatory
 Intestinal Macrophages                                   Cytokine Cascade
```
However, the nature of retrospective validation deserves critical scrutiny. DeepMind’s researchers knew the target gene, knew the relevant cell type (gut macrophages), and knew the precise biological locus before testing the model. In therapeutic development, that is not how drug discovery functions.

"In biology, computational models often excel when you hand them an exam where the answer key has already been published in *Nature*," says an immunologist at the Francis Crick Institute who worked alongside Lee on the original *ETS2* experiments. "The real challenge is prospective discovery. If you hand an algorithm fifty poorly characterized GWAS loci for systemic sclerosis, with no prior knowledge of whether the causal cell is an endothelial cell, a pericyte, or a pathogenic fibroblast, will it lead you to the right target? Or will it spit out ten compelling false positives that consume two years of wet-lab validation?"

In the original 2024 wet-lab study, identifying *ETS2* was only the first step. Lee’s team went further, demonstrating that MEK inhibitors—drugs originally developed for oncology—could suppress the toxic hyper-inflammation driven by *ETS2* in patient gut biopsies. However, the team also discovered that systematically blocking MEK caused acute toxicities in non-target tissues, prompting them to design cell-directed delivery systems to target the drug exclusively to intestinal macrophages.

AlphaGenome 2 cannot perform that downstream chemistry. It can map the lesion and nominate the cell type, but it remains blind to systemic pharmacology, off-target toxicity, and organism-level phenotypic feedback. It provides a sharper biological compass, but the wet lab must still traverse the terrain.

### The Compute Tax: Why the Atlas Vanished

When DeepMind released the first AlphaGenome in 2025, it accompanied the release with the AlphaGenome Atlas: a 1-petabyte open repository containing precomputed functional scores for all nine billion possible single-letter changes in the reference human genome. A researcher studying a rare pediatric mutation could query a website and immediately retrieve a score indicating whether that mutation likely disrupted gene expression or chromatin packaging.

With AlphaGenome 2, the universal atlas has disappeared.

The reason is mathematical. The original AlphaGenome was a parallel model: a single forward pass computed predictions across thousands of tissue and cell tracks simultaneously. Precomputing nine billion variants was computationally brutal, but finite.

AlphaGenome 2, by contrast, predicts only *one condition per call*. If researchers want to query nine billion variants across 500 distinct human cell types and cellular states, the compute requirement expands by nearly three orders of magnitude:

$$\text{Total Forward Passes} = 9 \times 10^9 \text{ variants} \times N \text{ cellular states}$$

For 500 states, that amounts to 4.5 trillion forward passes over a 1-million-base-pair context window using an 11-block transformer network.

DeepMind lead Žiga Avsec confirmed this operational bottleneck, noting that the increased computational demands make a comprehensive, single-cell, genome-wide atlas impractical with existing hardware.

Instead of an exhaustive nine-billion-variant catalog, DeepMind was forced to compromise. The company released precomputed scores for approximately 7 million common variants and known GWAS loci across 510 cellular conditions, accompanied by an enhancer-gene link map covering 640 primary cell types. For everything else—including private patient mutations, rare variants, and custom single-cell profiles—researchers must execute dynamic, on-demand inference via the cloud API.

This shift marks the arrival of a "compute tax" in modern single-cell genomics. While academic labs could previously download static flat files of the AlphaGenome Atlas to conduct independent, offline analyses on institutional clusters, querying AlphaGenome 2 at scale requires constant, high-throughput communication with DeepMind’s proprietary server infrastructure.

### Silicon Gatekeeping: The Battle Over Biological Infrastructure

The architectural shift from a static atlas to an on-demand conditional API highlights a contentious debate over who controls the computational infrastructure of life sciences.

DeepMind has divided access to AlphaGenome 2 along commercial lines:

- **Non-Commercial Academic Access** : Available immediately free of charge via the AlphaGenome API, subject to DeepMind's terms of service and usage quotas.
- **Commercial Industry Access** : Placed into an exclusive "private preview," designed for biotechnology and pharmaceutical corporations.
- **Agentic Workflows** : Integrated directly into Google Antigravity—Alphabet’s scientific agent workbench—allowing automated AI agents to access genomic databases and iteratively formulate drug design hypotheses.
- **Enterprise Partnerships** : Platforms like Genomics Limited’s Mystra will integrate the model to cross-reference with proprietary clinical trial biobanks.

Crucially, Google DeepMind has not released the model weights for AlphaGenome 2.

For academic geneticists and open-source computational biologists, this closed-weights posture represents an alarming break from genomic tradition. The Human Genome Project, the 1000 Genomes Project, and the Human Cell Atlas were constructed on an explicit mandate of absolute data and software availability.

"We are entering an uncomfortable territory where the foundational instruments of genomic interpretation are becoming proprietary black boxes governed by corporate APIs," says a director of a prominent European genomics institute. "If a pharmaceutical company identifies a novel oncology target using AlphaGenome 2, or if an academic lab uses it to nominate a pathogenic mechanism, the entire hypothesis rests on an algorithm whose internal weights cannot be independently inspected, hosted, audited, or reproduced locally."

Competing open-source models do exist. Frameworks like Enformer (developed by DeepMind alumni and published openly in 2021), Borzoi (developed by Calico and open-sourced), and recent foundation models like the Arc Institute's Evo and Stanford’s HyenaDNA provide accessible model weights that universities can run on their own hardware. Yet none of these open architectures match the scale of AlphaGenome 2’s 107-million-cell training corpus or its conditional FiLM modulation pipeline.

DeepMind defended its API-only rollout by pointing to the compute infrastructure required to execute conditional million-base inference, as well as biosecurity safeguards. The company explicitly stated in its launch materials that AlphaGenome 2 is an experimental research tool that has not been approved or validated for direct clinical or diagnostic use in human patients.

Nevertheless, the strategic direction is evident. DeepMind is no longer positioning its science branch simply as an academic research laboratory that periodically writes papers for *Nature*. It is constructing the computational backbone of modern medicine—a proprietary layer of intelligence that sits between raw sequencer output and downstream therapeutic development.

### From Single-Cell Mapping to "Virtual Cells"

Despite the computing bottlenecks and open-source anxieties, researchers inside DeepMind are already focused on what lies beyond static cell types.

The current version of AlphaGenome 2 models a cell as a static snapshot—a fixed 256-dimensional vector representing a specific state, such as an activated macrophage or a resting memory T cell. But biology is continuous, fluid, and dynamic. Cells change their states second by second in response to metabolic stress, cytokine signaling, tissue injury, and epigenetic aging.

DeepMind's internal roadmaps show researchers testing the integration of AlphaGenome 2 with larger multi-scale cellular simulations—a project known informally within the AI-for-science community as the race toward the "virtual cell".

```
The Virtual Cell Trajectory:
[ Static Variant Impact ] ──► [ Dynamic Trajectory Modeling ] ──► [ Complete In Silico Simulation ]
(AlphaGenome 1: Bulk)         (AlphaGenome 2: Single-Cell)        (Multi-Modal Virtual Cell)
- Identifies gross tissue     - Predicts variant function in      - Models temporal cellular shifts,
  averages; misses rare         discrete cell states; simulates     drug response, and multi-cellular
  pathological drivers.         lineage differentiation steps.      tissue interactions dynamically.
```
"Single-cell variant prediction is only stage one," explains an engineer working within DeepMind’s science division. "The real objective is predictive perturbation biology. You mutate an enhancer sequence in silico, and the model simulates not just the immediate drop in RNA transcription, but the downstream cascade: how chromatin condenses, how neighboring transcription factors dissociate, and how the entire cell transitions from a healthy lineage into a diseased phenotype over forty-eight hours."

Signs of this trajectory are already apparent in AlphaGenome 2's capacity to trace developmental lineages.

Outside of its immediate training domains, the model was tested on its ability to track continuous biological trajectories—specifically, how gene expression and chromatin structures evolve as human hematopoietic stem cells mature into erythrocytes, monocytes, and B-cell lineages. The network reproduced the gradual, non-linear activation and deactivation of lineage-defining genes without requiring discrete, compartmentalized retraining runs.

Additionally, the model's cross-modality imputation capabilities hint at a future where wet-lab experiments become vastly leaner. Because the network uncouples the cell state from its final outputs, a researcher who measures only single-cell gene expression (scRNA-seq) can use the model to predict unmeasured epigenetic features, such as single-cell chromatin accessibility (scATAC-seq) or DNA methylation patterns, across the identical cellular population.

This allows researchers to computationally fill in experimental blind spots, prioritizing expensive laboratory assays only when the AI signals a high-confidence structural alteration.

### The Frontier of Genetic Medicine

The launch of AlphaGenome 2 makes one reality clear: the era of treating the human genome as a static, linear text file is coming to an end.

For more than two decades, the life sciences operated under a profound paradox. Humanity possessed the technology to read every one of the 3.2 billion letters of our genetic code, but our ability to interpret those letters was restricted to the tiny two percent that codes for proteins. The remaining 98 percent—the non-coding regulatory switches, promoters, enhancers, silencers, and architectural insulators—remained an impenetrable dark matter.

AlphaGenome 2 brings that dark matter into clearer focus. By conditioning on the single cell, it recognizes that DNA is not a static blueprint; it is an interactive program that executes entirely different biological subroutines depending on whether it is running on the hardware of an immune cell, a neuron, or a cardiomyocyte.

The immediate implications for the biotechnology industry are tangible:

- **Accelerated Target Discovery** : Pharmaceutical companies can bypass years of preliminary non-coding GWAS triage, evaluating whether candidate disease variants act directly within target clinical tissues before initiating in vivo experiments.
- **Cell-Specific Therapeutic Design** : Knowing that a disease-associated mutation operates exclusively in a specialized sub-population—such as gut macrophages rather than all myeloid cells—enables the design of targeted drug-delivery conjugates that avoid systemic off-target toxicities.
- **Deciphering Rare Pediatric Diseases** : Non-coding mutations that cause severe developmental or metabolic disorders without hitting known genes can now be evaluated across hundreds of developmental cell states to find the exact lineage where normal transcriptional machinery broke down.

Yet, the investigative journey behind DeepMind’s newest tool leaves three urgent challenges in plain view.

The first is the **Validation Gap**. Reconstructing a known regulatory loop in thirty minutes demonstrates computational power; confirming that hundreds of uncharacterized mutations cause real clinical pathology requires prospective, blinded wet-lab trials that will take years to conduct. 

The second is the **Inference Barrier**. As genomic models incorporate more fine-grained cell states, spatial orientations, and temporal dimensions, the computational cost of running functional predictions is exploding. The inability to build a static single-cell variant atlas demonstrates that even Alphabet's data centers face hard mathematical constraints when simulating biological complexity.

The third is the **Sovereignty Question**. As foundational genomics transitions from open public repositories to proprietary enterprise APIs, the scientific community must confront who will ultimately own the interpretation of human biology.

For researchers like James Lee, standing over culture dishes in central London, AlphaGenome 2 is an undeniable asset. It solves in minutes calculations that once demanded years of experimental life. But as algorithms continue to chart the labyrinth of the non-coding genome, the seventy percent of causal disease mechanisms that still elude the model serve as a persistent reminder: computing biological predictions is fast, but deciphering the ultimate secrets of the living cell remains an ongoing scientific journey.

#### Key Technical Milestones

- **November 11, 2026** : Google DeepMind officially launches AlphaGenome 2, shifting genomics AI from bulk tissue averages to single-cell and cell-state conditioning.
- **Core Architecture** : Sequence-to-function U-Net utilizing a convolutional encoder, 11-block transformer, and dynamic Feature-wise Linear Modulation (FiLM) conditioned on a 256-dimensional cell-state embedding.
- **Training Foundation** : Over 107 million single-cell profiles across 68 human and mouse tissues, incorporating pseudobulk single-cell RNA and ATAC data from 36 distinct studies.
- **Validated Immune Benchmarks** : Identifies 30% of causal expression variants (sc-eQTLs) at 90% precision on the TenK10K dataset, compared to 16% for its predecessor; directional gene effect prediction rises from 23% to 43%.
- **Novel Epigenetic Readouts** : Incorporates DNA methylation prediction alongside existing metrics for gene expression, splicing junctions, chromatin accessibility, histone marks, and 3D contact topologies.
- **Zero-Shot Adaptability** : An external cell encoder permits custom single-cell RNA-seq datasets (down to 30–50 cells) to establish custom conditions on the fly without model retraining or parameter fine-tuning.
- **Ecosystem Integration** : Available immediately for non-commercial research via the AlphaGenome API; commercial private preview launched alongside scheduled integration into the Google Antigravity scientific workbench.
