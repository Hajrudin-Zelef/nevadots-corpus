---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-s-nemotron-for-science-challenges-larger-ai-models-on-cost-and-accuracy-hpcwire
title: "Nvidia's Nemotron for Science Challenges Larger AI Models on Cost and Accuracy - HPCwire"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-17"]
keywords: ["nemotron", "models", "nvidia", "model", "research", "data", "larger", "cost"]
source: https://www.hpcwire.com/2026/11/17/nvidias-nemotron-for-science-challenges-larger-ai-models-on-cost-and-accuracy/
source_anchor: ""
source_lines: [1, 27]
section: "NVIDIA_Hardware"
---
Nvidia says smaller AI models trained on a lab’s own data can do just as well, or even better, than much larger AI models on some scientific tasks. And they can cost a lot less to run.

The company has introduced Nemotron for Science – a new offering built for researchers to train their own AI agents for scientific work. It’s built around an open model that researchers can download and adapt using their own data. There’s also a new chemistry dataset and training tools to help with different kinds of research, from reading chemical diagrams and planning lab experiments to helping design new drugs.

The Nemotron model itself isn’t new. Nvidia has been developing the family of open AI models for some time, with the 120-billion-parameter Nemotron 3.5 Super VL introduced in October 2026.

Scientists don’t always need a huge AI model that can do a bit of everything. Sometimes they need one that’s really good at a particular job. And if they can train it on their own data, it might end up doing that job better than a much larger model.

Nemotron for Science aligns well with a bigger trend. Scientists are using AI agents for more than just analyzing data or answering questions. They want systems that can write code, run simulations, and help carry out actual research.

Nvidia believes specialized open models can do much of this work without the cost of running the largest AI systems. The company shared some examples in the fields of chemistry and life sciences. These aren’t general tests of AI intelligence. They’re specific tasks where the models can be trained on specialized data and their results measured against known outcomes.

Lila Sciences is one of the companies that has been trying this out. It runs automated labs where scientists work on developing new materials. The company used results from its own experiments to train Nemotron further, and managed to get its accuracy in making quantum dots up to 61%. That’s compared with 54% for GPT-6 Astra. Nvidia says Lila’s version also costs 10 to 20 times less to run.

At Terray Therapeutics, the work is more about getting useful information out of existing research. The company trained Nemotron to read chemical diagrams and figure out what went into a reaction, what came out, and under what conditions. It managed to identify about 76% of the annotated reactions in the RxnScribe test, better than previously published results.

Therna is using the model for a different problem: designing mRNA therapies. Its version was nearly twice as accurate as frontier models at predicting how much protein would be produced. And when scientists compared its explanations with those from the original Nemotron model, they preferred the customized version 96% of the time.

Of course, these figures come from Nvidia and its partners. The comparisons involve different tasks and benchmarks. They show what specialized models might be capable of. However, they don’t necessarily prove that smaller models are better across scientific research.

Nvidia is also looking to bring these models into major scientific research centers. Argonne National Laboratory is integrating Nemotron into StructBioReasoner – a system that uses multiple AI agents to help develop protein-based treatments for cancer and other diseases. In Japan, RIKEN has made Nemotron available to researchers through its RIKYU AI supercomputing platform.

At KTH Royal Institute of Technology, researchers are using Nemotron to fix problems with GROMACS, software used for molecular dynamics simulations. They built an AI agent that can work through these problems on its own. In one overnight run on a single Nvidia DGX Spark system, it managed to fix 81% of the test cases. GROMACS was then used to check whether the fixes actually worked.

To get more researchers working with Nemotron, Nvidia has made LitmusBench v0.2 available, a dataset with nearly 50,000 chemistry questions. There are also tools to help scientists train the model for their own research. Both Nemotron 3.5 Super VL and the dataset can be downloaded from Hugging Face.

Researchers don’t necessarily need a big supercomputer to use it either. Nvidia says the model can run locally on a DGX Spark system with 128GB of memory, although it’s also being used at much larger computing centers.
