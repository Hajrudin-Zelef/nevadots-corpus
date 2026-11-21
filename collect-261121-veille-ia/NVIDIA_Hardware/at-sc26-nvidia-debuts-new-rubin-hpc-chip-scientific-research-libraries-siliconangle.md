---
id: collect-261121-veille-ia/nvidia-hardware/at-sc26-nvidia-debuts-new-rubin-hpc-chip-scientific-research-libraries-siliconangle
title: "At SC26, Nvidia debuts new Rubin HPC chip, scientific research libraries - SiliconANGLE"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "scientific", "rubin", "research", "chip", "data", "sc26", "libraries"]
source: https://www.siliconangle.com/2026/11/16/nvidia-debuts-new-rubin-hpc-chip-scientific-research-libraries/
source_anchor: ""
source_lines: [1, 55]
section: "NVIDIA_Hardware"
---
### At SC26, Nvidia debuts new Rubin HPC chip, scientific research libraries

Nvidia Corp. today introduced a set of offerings designed to speed up scientific research across a range of fields.

The lineup is headlined by Rubin HPC, an upcoming graphics card with significantly more cores than the company’s current flagship chip. It’s joined by more than a half-dozen software tools geared toward scientists and engineers. The technologies made their debut at the SC26 high-performance and supercomputing conference in Chicago.

### Rubin HPC for scientific workloads

Nvidia’s flagship Rubin graphics processing unit features 898 Tensor cores, transistor clusters specifically optimized for large language models. It also includes over 28,000 CUDA cores. Those circuits can run not only LLMs but also other workloads such as scientific simulations.

The Rubin HPC (pictured), the upcoming chip that Nvidia previewed today, will feature more than 50,000 CUDA cores. It will also ship with a new iteration of NVLink, the interconnect that Nvidia’s GPUs use to exchange data with central processing units. The current version of the technology can move 3.6 terabits per second per graphics card.

Nvidia stated that Rubin HPC will also include a “sparse data acceleration” feature. Sparse data is the technical term for datasets that contain a large number of zeros. Many scientific projects generate such data, particularly in fields such as biology and astrophysics. A telescope, for example, might use zeros to represent sections of space without any visible stars.

“For agents tackling science and engineering, faster simulation means faster discovery,” said Ian Buck, Nvidia’s vice president of hyperscale and high-performance computing. “Rubin HPC accelerates the simulations agents use to test ideas and guide their next steps, helping scientists and engineers explore more possibilities and bring breakthroughs closer.”

Nvidia plans to ship Rubin HPC in 2028. In the meantime, customers can speed up their scientific workloads using the software tools that debuted alongside the chip today.

### Faster astrophysics research

Nvidia ships its GPUs with a collection of libraries called CUDA-X that is designed to ease scientific research projects. The company’s SC26 announcement introduced several new additions to the suite.

Astrophysicists are gaining access to cuPhoton.xBeam, a library that makes it easier to analyze data from the Event Horizon Telescope network. It’s a collection of observatories used study black holes. According to Nvidia, cuPhoton.xBeam can perform data analysis tasks that historically took a month in eight minutes. It’s rolling out alongside cuPhoton.xLens, a library for simulating the plasma that fusion reactors use to generate electricity.

On the biology research side, Nvidia is open-sourcing cuSpa. It speeds up cell polygon calculations, which are used to study the structure of cells and the manner in which they interact with one another. Plus, an existing cellular analysis tool called rapids-singlecell is now available to Nvidia users in a containerized format.

### LLM-driven research

Nvidia’s new biology research tools are joined by an AI bundle called Nemotron for Science. It’s built on Nemotron 3.5 Super VL, an open-source LLM the company introduced last month. The model features 124 billion parameters and activates about a 10th of them at any given time. It’s partly based on Mamba-2, a more hardware-efficient alternative to the ubiquitous Transformer model architecture.

Nemotron for Science pairs the LLM with a set of pre-packaged post-training workflows. Researchers can use them to optimize Nemotron 3.5 Super VL for specific scientific tasks. Additionally, Nvidia has added in nearly 50,000 chemistry-related questions. They can be used to post-train AI models and evaluate the capabilities of models that have already gone through the process.

“Nemotron for Science gives researchers open models, scientific data and post-training recipes to build specialized agents that reason through scientific problems and use CUDA-X accelerated tools to turn ideas into discoveries,” Buck said.

### Quantum algorithms

Quantum computers rely on a conventional processor to manage their qubits. Nvidia provides a set of software tools called CUDA-Q that makes it easier to link quantum hardware to its GPUs and CPUs. The company today expanded the suite with new building blocks for developing quantum software and an algorithm that can mitigate qubit errors.

Nvidia is teaming up with IBM Corp. and the U.S. Energy Department’s Fermilab to create a “modular quantum codesign platform.” It will combine the chip giant’s DGX B200 systems with quantum hardware from IBM. According to the companies, the goal is to create an environment that researchers can use to develop new qubit error correction algorithms and other quantum software.

##### Image: Nvidia

# A message from John Furrier, co-founder of SiliconANGLE:

Support our mission to keep content open and free by engaging with theCUBE community. **Join theCUBE’s Alumni Trust Network**, where technology leaders connect, share intelligence and create opportunities.

- **15M+ viewers of theCUBE videos** , powering conversations across AI, cloud, cybersecurity and more
- **11.4k+ theCUBE alumni** — Connect with more than 11,400 tech and business leaders shaping the future through a unique trusted-based network

### Are you an AWS customer? Support SiliconANGLE financially by buying your AWS services from our Marketplace portal page and links: https://siliconangle.com/aws-marketplace/


##### **About SiliconANGLE Media**

Founded by tech visionaries John Furrier and Dave Vellante, SiliconANGLE Media has built a dynamic ecosystem of industry-leading digital media brands that reach 15+ million elite tech professionals. Our new proprietary theCUBE AI Video Cloud is breaking ground in audience interaction, leveraging theCUBEai.com neural network to help technology companies make data-driven decisions and stay at the forefront of industry conversations.
