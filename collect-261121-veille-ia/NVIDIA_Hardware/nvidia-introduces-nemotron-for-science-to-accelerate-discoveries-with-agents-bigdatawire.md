---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-introduces-nemotron-for-science-to-accelerate-discoveries-with-agents-bigdatawire
title: "NVIDIA Introduces Nemotron for Science to Accelerate Discoveries with Agents - BigDATAwire"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nemotron", "scientific", "nvidia", "science", "agents", "models", "open", "researchers"]
source: https://www.hpcwire.com/bigdatawire/this-just-in/nvidia-introduces-nemotron-for-science-to-accelerate-discoveries-with-agents/
source_anchor: ""
source_lines: [1, 49]
section: "NVIDIA_Hardware"
---
The new Fire phone that Amazon launched this week looks like your ordinary black smartphone,...

## Off the Wire Press Releases

*New Open Dataset and Post-Training Recipes Help Researchers Adapt Nemotron 3.5 Super VL for Life Sciences and Chemistry*

Nov. 16, 2026 — NVIDIA today introduced NVIDIA Nemotron for Science, bringing together scientific applications of its NVIDIA Nemotron 3.5 Super VL open model, a new open chemistry dataset and post-training recipes to help researchers build specialized agents for scientific discovery.

Built on Nemotron 3.5 Super VL, Nemotron for Science helps researchers develop agents that reason through scientific problems and put specialized research tools to work. With post-training, scientists can adapt the model to specialized tasks such as conducting experiments for chemistry, protein design and more.

Part of Nemotron for Science, LitmusBench v0.2 is a new open dataset with nearly 50,000 questions for training and evaluating language models on chemistry reasoning. Drawing on molecular properties and experimental drug discovery data, it helps researchers improve models’ ability to predict how molecules behave, including their solubility and how the human body processes them.

Life sciences companies Lila Sciences, Terray Therapeutics and Therna are using and customizing NVIDIA Nemotron 3.5 Super VL for science applications. Use cases include extracting chemical knowledge, planning experiments and designing therapeutics and laboratory protocols. Major research centers like Argonne and Jülich Supercomputing Centre are integrating it into their drug and scientific discovery AI agents.

“Accelerated computing is transforming how scientists tackle the world’s most complex challenges,” said Ian Buck, vice president of hyperscale and high-performance computing at NVIDIA. “Nemotron for Science gives researchers open models, scientific data and post-training recipes to build specialized agents that reason through scientific problems and use CUDA-X accelerated tools to turn ideas into discoveries.”

**Post-Training Nemotron Open Models for Scientific Discovery**

Researchers can post-train Nemotron open models with domain-specific data to improve performance on specialized scientific tasks. They can integrate the resulting model into agent workflows that interpret scientific information, generate code and call computational tools. These workflows can use NVIDIA CUDA-X accelerated tools to perform scientific computation.

Terray Therapeutics post-trained Nemotron to extract reactants, products and reaction conditions from scientific diagrams, surpassing previously published results on two benchmarks. On the RxnScribe test, it recovered about 76% of annotated reactions under the benchmark’s matching criteria, helping turn published chemistry discoveries into structured data for synthesis planning and scientific agents.

Lila Sciences used Nemotron to design execution-ready laboratory protocols for its automated labs, matching frontier-level performance without requiring additional fine-tuning. Post-training with laboratory feedback nearly doubled the model’s accuracy for making quantum dots — nanoscale crystals used in screens and solar panels — to 61%, exceeding GPT-6 Astra’s 54% at 10-20x lower inference cost. These gains help agents and materials scientists translate desired material properties into synthesis recipes, advancing the discovery of new materials.

Therna collaborated with NVIDIA to post-train Nemotron for agentic mRNA therapeutic design, nearly doubling accuracy in predicting protein-expression ranges over frontier models. Scientists preferred its explanations over those from the base model in 96% of comparisons. These gains could help scientists tailor mRNA designs for the desired protein production in specific cell types as they scale to human use.

**Researchers Speed Science With Nemotron Models and Datasets**

Leading research institutions are using NVIDIA Nemotron models and datasets to advance scientific discovery at every scale, from individual researchers’ desktops to labs and supercomputing centers.

By helping researchers rapidly design and evaluate novel molecules across the periodic table, the work has the potential to open new pathways for applications in critical materials and minerals, national security and cancer treatment.

Argonne is integrating Nemotron into its StructBioReasoner multi-agent system to design and evaluate protein-based therapeutic candidates for treatment of cancer and other diseases.

KTH Royal Institute of Technology has built an AI agent on Nemotron that repairs broken GROMACS molecular dynamics setups on its own. In one overnight run on a single NVIDIA DGX Spark system, the agent fixed 81% of test cases with reasoning enabled, and every fix was checked independently by GROMACS itself.

RIKEN has made NVIDIA Nemotron, including Nemotron 3.5 Super VL, available on its RIKYU AI supercomputing platform through RiVault, its inference gateway, enabling researchers to access the models for scientific research and the development of accelerated applications.

**Availability**

__NVIDIA Nemotron 3.5 Super VL__ and the __LitmusBench v0.2__ chemistry dataset are available now on Hugging Face. The Nemotron 3.5 Super VL model was also optimized to run locally on a 128GB __NVIDIA DGX Spark system__.

*Learn more in this* __technical blog__ *on post-training NVIDIA Nemotron to increase accuracy across scientific domains.*

**About NVIDIA**

NVIDIA (NASDAQ: NVDA) is the world leader in AI and accelerated computing.

*Source: NVIDIA*
