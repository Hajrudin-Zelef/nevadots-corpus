---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-for-science-model-and-license-details-neoteo
title: "NVIDIA Nemotron for Science: Model and License Details | NeoTeo"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "license", "model", "nemotron", "scientific", "dataset", "science", "details"]
source: https://www.neoteo.com/en/nvidia-introduced-nemotron-for-science-with-model-and-data-details?locale=en
source_anchor: ""
source_lines: [1, 33]
section: "NVIDIA_Hardware"
---
NVIDIA introduced Nemotron for Science at SC26 on November 16, 2026. The package brings together Nemotron 3.5 Super VL, scientific evaluations, the LitmusBench v0.2 chemistry dataset and post-training recipes for scientific agents. NVIDIA said the model and dataset were available on Hugging Face. NVIDIA’s announcement

For developers, the practical details are in the model’s deployment configuration and the license attached to each dataset record.

## What the package includes

NVIDIA describes Nemotron for Science as a set of resources for building agents for scientific work, including chemistry and life sciences. LitmusBench v0.2 is described as nearly 50,000 chemistry questions for training and evaluation. The package also includes scientific evaluations and recipes for post-training models on specialized data.

For background on the underlying model’s earlier release, see our coverage of Nemotron 3.5 Super VL.

## What the BF16 model card specifies

NVIDIA’s model card describes the BF16 version of Nemotron 3.5 Super VL as a vision-language model that accepts text, images and video and produces text. It lists 120 billion total parameters, of which 12 billion are active, and a context window of up to 1 million tokens. In deployment, the context may be smaller depending on available GPU memory and concurrent use. The BF16 model card

For that BF16 artifact, NVIDIA lists deployment configurations of **4× H100 or 2× GB200**. The company separately said Nemotron 3.5 Super VL was optimized for local use on a 128 GB NVIDIA DGX Spark.

The model card identifies OpenMDW-1.1 as the model’s license and describes it as ready for commercial or non-commercial use. That is a separate consideration from the hardware needed to deploy the BF16 version.

## LitmusBench licensing varies by record

LitmusBench v0.2 does not use one license for every question. NVIDIA’s license file assigns CC-BY-SA-4.0 to questions containing ChEMBL data and OpenMDW-1.1 to other questions. Each record’s `question_license` field identifies the applicable term. The license file also names ChEMBL, Biogen and ExpansionRx as source data. NVIDIA’s dataset license file

| Question type | License | Record field identifying the license | 
| Questions containing ChEMBL data | CC-BY-SA-4.0 | `question_license` | 
| Other questions | OpenMDW-1.1 | `question_license` | 

The listed dataset revision contains `train.jsonl` and `validation.jsonl`; its Hugging Face page labels the content as English text in JSON format. Dataset revision details

## Scientific applications NVIDIA reported

NVIDIA described several collaborators’ uses of Nemotron. Terray Therapeutics post-trained the model to extract chemical reactions from scientific diagrams; NVIDIA reported about 76% recovery of annotated reactions on the RxnScribe test. After laboratory-feedback post-training by Lila Sciences, NVIDIA reported 61% accuracy on a quantum-dot task, compared with 54% for GPT-6 Astra. For Therna’s mRNA therapeutic-design work, NVIDIA said scientists preferred the model’s explanations over those from the base model in 96% of comparisons.

NVIDIA also reported that a KTH Royal Institute of Technology agent fixed 81% of test cases in one overnight run on a single DGX Spark, with reasoning enabled. GROMACS checked each fix. NVIDIA reported these collaborator applications and results.
