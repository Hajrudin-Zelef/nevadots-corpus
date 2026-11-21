---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-gpu-and-nemotron-for-science-at-sc26
title: "Nvidia Unveils Rubin HPC GPU and Nemotron for Science at SC26"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "rubin", "nemotron", "science", "scientific", "sc26", "vera", "agents"]
source: https://techstrong.ai/articles/nvidia-unveils-rubin-hpc-gpu-and-nemotron-for-science-at-sc26/
source_anchor: ""
source_lines: [1, 53]
section: "NVIDIA_Hardware"
---
Nvidia Unveils Rubin HPC GPU and Nemotron for Science at SC26

4.5 min readPublished On: November 16, 2026By Jaime Hampton

TL;DR — Key Takeaways

Nvidia introduced Nemotron for Science at SC26, combining an open AI model, chemistry dataset and post-training tools for developing scientific research agents.

Nvidia unveiled the Rubin HPC GPU with more than 50,000 CUDA cores and a Vera CPU rack system with 192 Vera CPUs, with availability planned for 2028 and 2027, respectively.

Nvidia expanded its CUDA-X libraries for scientific research, citing major performance improvements in astronomical data analysis and cellular biology applications.

With Jensen Huang out and its future leadership uncertain, Nvidia is facing an unusually turbulent moment. But at SC26 in Chicago, the company is making headlines for something more familiar: its latest advances in AI and high performance computing aimed at accelerating scientific discovery.

On Monday, Nvidia introduced Nemotron for Science, a collection of resources for developing AI agents that can assist researchers with complex scientific problems. The company also unveiled new high performance computing hardware and expanded its CUDA-X software libraries, continuing its technology roadmap as interim CEO Colette Kress prepares for a closely watched earnings call Tuesday.

Nemotron for Science Adds Tools for AI Agents

As one of the industry’s largest supercomputing conferences got underway in the Windy City, Nvidia introduced Nemotron for Science, a suite of AI models, datasets and tools for building specialized agents for scientific discovery.

Nemotron for Science includes Nvidia’s Nemotron 3.5 Super VL open model and the LitmusBench v0.2 dataset, both available on Hugging Face, along with post-training tools that help researchers adapt the model for tasks such as chemistry experiments, protein design and molecular analysis.

LitmusBench contains nearly 50,000 questions for training and evaluating AI models on chemistry reasoning. Nvidia said the dataset could help researchers improve models’ ability to predict how molecules behave, including their solubility and how the human body processes them.

Several research institutions are using Nemotron in their scientific work. Argonne National Laboratory is integrating the model into StructBioReasoner, a multi-agent system that designs and evaluates protein-based therapeutic candidates for cancer and other diseases.

At Sweden’s KTH Royal Institute of Technology, researchers developed a Nemotron-based agent that automatically repairs faulty GROMACS molecular dynamics simulation setups. In an overnight test using a single Nvidia DGX Spark system, the agent fixed 81% of test cases with reasoning enabled, with each correction verified by the simulation software.

New Rubin HPC GPU and Vera CPU Racks Unveiled

While scientific AI agents can help researchers explore new materials and molecules, evaluating their ideas still requires computationally intensive simulations.

To support that simulation work, Nvidia unveiled Rubin HPC, a new GPU designed to accelerate scientific computing, engineering workloads and data analysis. Part of the company’s Vera Rubin platform, the processor features more than 50,000 CUDA cores, a record for a single Nvidia GPU and nearly twice the 28,672 cores in the standard Rubin GPU.

Nvidia unveiled the Rubin HPC GPU at SC26 on Monday (Image courtesy of Nvidia)

Rubin HPC also includes specialized acceleration for sparse data operations, which are common in scientific computing. Nvidia said the GPU is designed to keep its processing cores supplied with data during intensive HPC workloads, including climate modeling, engineering simulations and genomic analysis. Nvidia plans to make Rubin HPC available in 2028.

For CPU-intensive HPC workloads, Nvidia introduced a rack-scale Vera system combining 192 Vera CPUs with approximately 300TB of memory and 1PB of storage. The liquid-cooled system has more than 200TB per second of aggregate peak memory bandwidth, a specification that could help ease data-transfer bottlenecks in large-scale scientific computing. Dell, HPE, Lenovo and Supermicro plan to begin offering Vera CPU racks in early 2027, according to Nvidia.

In an OpenFOAM airflow simulation benchmark, Nvidia reported that Vera CPUs achieved 1.9 times the performance and 2.4 times the performance per watt of x86 systems. Germany’s DKRZ climate computing center has selected Vera CPUs for the CPU portion of its HLRE-5 supercomputer, which will also use Nvidia GB200 Grace Blackwell systems. Planned for service in early 2028, HLRE-5 will be able to run two global climate simulations simultaneously at 5-kilometer resolution.

CUDA-X Expands as CUDA Marks 20 Years

Nvidia also expanded its CUDA-X software libraries to give researchers and agents more tools for processing scientific data and performing specialized calculations. The expanded libraries support research in fields including astrophysics, fusion energy, drug discovery and cellular biology.

One example is cuPhoton.xBeam, a module in Nvidia’s CUDA-X cuPhoton toolkit. Nvidia said the software reduced the time needed to correlate observations from the Event Horizon Telescope from more than a month to just eight minutes. Another module, cuPhoton.xLens, lets researchers analyze plasma simulations as they run and can be paired with Nemotron-powered agents to help guide fusion research.

For molecular research, Nvidia said its cuPotential library can accelerate existing simulation software by up to three times. The company also made its cuSpa library open source. Nvidia said the software accelerated calculations used to create spatial maps of cells by 900 times compared with CPU processing in work at Memorial Sloan Kettering Cancer Center.

The announcements coincide with the 20th anniversary of CUDA, Nvidia’s parallel computing platform that makes it possible to use GPUs to accelerate AI, scientific simulations and other demanding applications. Its CUDA-X libraries provide specialized tools built on that platform. Developed by Nvidia VP Ian Buck and his team, building on Buck’s earlier research at Stanford, CUDA was introduced to the HPC community at SC06 in 2006, helping open up the use of GPUs beyond their traditional role in rendering graphics.

One of CUDA’s most significant milestones came in 2012, when Alex Krizhevsky, Ilya Sutskever and Geoffrey Hinton, all then at the University of Toronto, used CUDA to train AlexNet, an image-recognition neural network, on just two Nvidia GPUs. AlexNet won that year’s ImageNet competition, helping establish GPU-accelerated deep learning as a foundation for later advances in AI, including today’s image-generation models.
