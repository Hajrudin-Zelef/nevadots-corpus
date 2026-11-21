---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-unveils-rubin-hpc-chip-50-000-cuda-cores-built-for-scientific-computing-launching-in-2028-big
title: "Nvidia Unveils Rubin HPC Chip: 50,000 CUDA Cores Built for Scientific Computing, Launching in 2028 — BigGo Finance"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "data", "rubin", "cores", "scientific", "computing", "chip", "cuda"]
source: https://finance.biggo.com/news/dade2d6e-6ed9-461e-83e1-7eb823e0617d
source_anchor: ""
source_lines: [1, 42]
section: "NVIDIA_Hardware"
---
# Nvidia Unveils Rubin HPC Chip: 50,000 CUDA Cores Built for Scientific Computing, Launching in 2028

Nvidia is extending the battle over AI infrastructure from model-training grounds into laboratories and supercomputing centers. On November 16, at the SC26 supercomputing conference in Chicago, the chip giant unveiled "Rubin HPC," a next-generation GPU for scientific computing, engineering simulation, and data analysis, while also disclosing advances in three areas: CUDA-X software libraries, photonic interconnects, and quantum computing—an effort to bring GPU acceleration, AI agents, and traditional high-performance computing (HPC) under a single technology stack.

The core hardware of the announcement, Rubin HPC, is a new member of the Vera Rubin platform. A single GPU integrates more than 50,000 CUDA cores, setting a record for the number of cores in an Nvidia GPU. It also features sparse-data acceleration and uses seventh-generation NVLink interconnect technology to boost data transfer efficiency between GPUs. Nvidia said the product is expected to launch in 2028.

Nvidia's assessment of scientific computing workloads is this: tasks such as climate modeling, genomic analysis, and financial risk calculation require repeatedly extracting scattered information from massive datasets, performing computations, and iterating—simply stacking up compute cores is not enough. Data movement capability must keep pace, or cores will sit idle waiting for data. Rubin HPC's optimization therefore focuses on sparse matrix operations, stencil computations, sorting, and reductions—common scientific computing operations—striving to balance data movement with compute power.

What Nvidia repeatedly emphasized is the shift in demand brought about by AI agents. When agents can autonomously propose hypotheses, invoke specialized software to run simulations, and then adjust research directions based on results, the number of candidate materials, molecular structures, and engineering designs that can be explored will increase substantially—and the verification step relies heavily on simulation speed. Nvidia hopes to use hardware-software co-design to shorten the cycle from posing a question to validating a discovery.

On the software side, the expansion of the CUDA-X libraries is key to making scientific tools genuinely "usable" by agents. The tools announced include cuPhoton, cuPotential, and cuSpa, covering astrophysics, materials science, drug discovery, and single-cell biology.

The astrophysics case is the most striking. The Event Horizon Telescope (EHT) links radio telescopes worldwide into a virtual telescope with an effective diameter equal to Earth's, generating petabytes of data per observation, with traditional processing taking months. Nvidia says the cuPhoton.xBeam module uses GPUs to accelerate the correlation processing of radio data, compressing analysis that originally took more than a month into 8 minutes. The tool has been used to analyze observation data of black-hole jets J1924 and 3C279, and has advanced real-time image reconstruction of the black hole at the center of galaxy Messier 87.

Performance gains in other disciplines are likewise measured in multiples, as shown in the table below.

| Tool/Framework | Application Area | Performance Gain | Notes | 
|---|---|---|---|
| cuPotential | Drug discovery | Molecular simulation software up to 3x faster | Developed based on work from the Flatiron Institute of the Simons Foundation and D. E. Shaw Research | 
| Daviator | Materials science | Quantum-mechanics calculations up to 22x faster, costs reduced by up to 75% | Batched GPU compute kernels for VASP | 
| cuSpa | Biomedicine | Cell polygon computation up to 900x faster than CPU | Newly open-sourced; Memorial Sloan Kettering Cancer Center involved in application | 
| rapids-singlecell | Single-cell data analysis | Therna Biosciences data processing 200x faster | Separately, Basecamp Research used GPUs to accelerate the HMMER library, with benchmark tests up to 84x faster | 

Note: The above figures are test or application results disclosed by Nvidia during SC26.

In fusion research, cuPhoton.xLens allows researchers to extract and analyze data synchronously while plasma simulations are running, without waiting for experiments or computations to finish entirely; combined with locally running Nemotron agents, results can be analyzed interactively and simulation directions adjusted in real time, providing reference for the design of fusion reactor target chambers and cavities.

Network interconnect and quantum computing are the two pieces Nvidia is adding to complete the supercomputing infrastructure puzzle. Cloud service provider Lambda is putting Nvidia's co-packaged optics (CPO) technology into production environments, used to connect AI factories equipped with GB300 NVL72 systems and more than 10,000 GPUs. Nvidia says Lambda's tests show that, compared with traditional pluggable optical modules, using photonic switches reduces rack-level network latency by about 4% and pod-level latency by up to about 10%, with comparable throughput; photonic technology packages the optical engine together with the switch chip, enabling up to 5x energy-efficiency gains and up to 15x reliability gains, while reducing the number of lasers to one-quarter of traditional solutions. For clusters that routinely scale to tens of thousands of GPUs, a network failure can leave thousands of GPUs simultaneously waiting idle for data—latency, power consumption, and reliability directly determine compute utilization efficiency.

On quantum computing, Nvidia chose to enter through workflow management, extending Slurm to support quantum processing units (QPUs) within traditional supercomputing workflows. Over the past five years, the number of QPUs deployed at supercomputing centers has grown nearly 7x, and nearly 30 supercomputing centers worldwide are currently deploying more than 50 quantum processors. Since many supercomputing centers already use Slurm to schedule CPU and GPU tasks, Nvidia wants researchers to be able to uniformly schedule classical and quantum resources in a familiar environment. The update is based on collaboration with IBM, Pasqal, and the quantum computing community, supports interfaces such as QRMI and QDMI, and has been adopted by IBM, IQM, Quantinuum, and Pasqal; Slurm 26.11 is expected to be released later in November.

The CUDA-Q platform also made progress: the new TCBP-JOBS quantum error-correction decoding algorithm is up to 13,000x faster than industry-standard algorithms; Fermi National Accelerator Laboratory used the platform to shorten work that originally took about five months to three weeks. In addition, QunaSys used 1,024 Hopper GPUs to simulate more than 300 million quantum operations, achieving chemical accuracy in a tetrahydride model; Nvidia researchers used Germany's JUPITER supercomputer to simulate the operation of 104 qubits under real noise conditions, to test future fault-tolerant quantum architectures.

SC26 coincides with the 20th anniversary of CUDA's release. In November 2006, at SC06, Nvidia promoted this GPU programming platform to the scientific computing community, moving GPUs from graphics processing toward general-purpose parallel computing. Nvidia says more than 11 million developers worldwide now use CUDA, and CUDA-X has more than 1,000 libraries. A University of Illinois team once used CUDA to accelerate molecular dynamics simulations advancing research on HIV capsid structure; University of Toronto researchers trained AlexNet on two GPUs; and AlphaFold 2, which won the 2024 Nobel Prize in Chemistry, likewise relied on CUDA for GPU computation.

The Vera CPU rack-scale platform for high-performance computing now has a launch timetable. Nvidia announced that the platform is expected to be supplied starting in early 2027 by Dell Technologies (DELL), Hewlett Packard Enterprise (HPE), Lenovo (0992.HK), and Super Micro Computer (SMCI). A single rack integrates 192 Vera CPUs, interconnected via Nvidia Quantum-X800 InfiniBand networking, equipped with about 300TB of memory and 1PB of storage, with a liquid-cooled design and peak aggregate memory bandwidth exceeding 200TB/s; target workloads are data-intensive tasks such as climate modeling and molecular dynamics.

Judging by product cadence, Nvidia's ambitions have moved beyond the GPU market for AI training and inference: scientific computing, engineering simulation, quantum computing, and related infrastructure are all being brought into its domain. As AI agents shift from information-processing tools to research assistants capable of invoking specialized software and executing complex tasks, the collaborative efficiency among GPU compute, scientific software libraries, and high-speed interconnects will become a key variable in the next phase of high-performance computing competition. For the market, Rubin HPC's 2028 launch means limited near-term revenue contribution, but the expansion of CUDA-X libraries and the ecosystem positioning of Slurm and CUDA-Q are extending Nvidia's moat from hardware into the research workflow itself.

Once added, BigGo Finance appears first in Google Search Top Stories, so you get the broadest, most up-to-the-minute, and most comprehensive global financial news first.

Add preferred source
