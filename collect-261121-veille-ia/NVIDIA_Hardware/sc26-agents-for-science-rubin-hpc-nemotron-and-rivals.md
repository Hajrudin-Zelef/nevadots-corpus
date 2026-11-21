---
id: collect-261121-veille-ia/nvidia-hardware/sc26-agents-for-science-rubin-hpc-nemotron-and-rivals
title: "SC26 agents for science: Rubin HPC, Nemotron and rivals"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["science", "nvidia", "november", "nemotron", "rubin", "model", "agents", "sc26"]
source: https://thefrontierwire.com/articles/sc26-agents-for-science-rubin-hpc-nemotron/
source_anchor: ""
source_lines: [1, 183]
section: "NVIDIA_Hardware"
---
NVIDIA used SC26, the supercomputing conference in Chicago, to present a full stack for scientific AI agents, and most of what researchers can use from it today is software. On 16 November 2026, Ian Buck, NVIDIAâs vice president of hyperscale and high-performance computing, announced Nemotron for Science, a new Rubin HPC GPU, an expanded Vera CPU platform and a quantum-computing collaboration with IBM and Fermi National Accelerator Laboratory. The open model, the chemistry dataset, the compilers, several libraries and a quantum error-correction decoder are available now, by NVIDIAâs account. The Vera CPU racks are expected from early 2027 and Rubin HPC in 2028. The pitch tying it together, in NVIDIAâs words, is that agents ârely on simulations to test those ideas and guide their next steps,â so faster simulation means faster discovery.

NVIDIA was not alone in claiming scientific work that week. OpenAI launched a science blog on 15 November, published a usage study of scientists and expanded free access on 16 November, and on 17 November called GPT-6.2 Astra âthe best model in the world for scientists.â Alibabaâs Qwen4-Max launch on 16 November led its long-horizon section with a lab-synthesised coolant molecule and a drug-design run. Anthropic had shipped Claude Fable 5.2 on 9 November with physics, protein and mathematics results. And Moonshot AIâs international director told Web Summit that science is where the âreal excitementâ is, as a partner moved its Kimi model into a biomedical research agent.

This survey maps those announcements by layer: models, data and evaluations, tools and libraries, scheduling, compute and access. The Frontier Wire has not tested any of the products. It reads the companiesâ own releases, Hugging Face records, Artificial Analysis evaluation data and, where no primary source exists, named press reports. Performance claims are attributed to the company that made them.

**TL;DR**


- Shipping now, per NVIDIA: Nemotron 3.5 Super VL with the LitmusBench v0.2 chemistry dataset, HPC SDK 26.11, CUDA-Q QEC 0.9.0, open-source cuSpa and the DGX Spark HPC Developer Kit.
- Coming: Slurm 26.11 with quantum scheduling later in November, Vera CPU racks from early 2027, the DKRZ HLRE-5 system in early 2028 and Rubin HPC in 2028.
- Model labs are competing on science claims, mostly self-reported. On one independent test, Artificial Analysisâs Terminal-Bench-Science run, GPT-6.2 Astra scored 78.1%, Fable 5.2 73.3%, Kimi K3.1 Pro 34.3% and Qwen4-Max 23.3%.
- DP Technology, a Beijing AI-for-science start-up, appears in both Alibabaâs and Moonshotâs science stories, a sign that domain platforms can choose among frontier models.

## The stack, layer by layer

The announcements are easier to compare when sorted by the job each does in an agentâs loop. An agent for science needs a model that reasons and calls tools, tools that run the actual computation, a scheduler that gives it hardware, and the hardware itself. The table below places each announcement from 5 to 17 November in that taxonomy. âShippingâ means the company says it can be used or bought now; âannouncedâ means a stated future date or a plan.

| Organisation | Announcement | Date | Layer | Status | Availability as stated | 
|---|---|---|---|---|---|
| NVIDIA | Rubin HPC GPU | 16 Nov | Compute (GPU) | Announced | âExpected to be available in 2028â | 
| NVIDIA | Vera CPU rack-scale platform for HPC | 16 Nov | Compute (CPU) | Announced | Racks from Dell, HPE, Lenovo and Supermicro âbeginning in early 2027â | 
| NVIDIA | HPC SDK 26.11; Fortran compiler public beta | 16 Nov | Compilers | Shipping | SDK âavailable nowâ; compiler in public beta | 
| NVIDIA | Nemotron for Science (Nemotron 3.5 Super VL, LitmusBench v0.2, recipes) | 16 Nov | Model, data | Shipping | Model and dataset âavailable now on Hugging Faceâ | 
| NVIDIA | CUDA-X libraries: cuPhoton, cuPotential, cuSpa, Daviator, DAQIRI and others | 16 Nov | Tools | Mixed | cuSpa ânewly open sourceâ; Daviator âcoming to NVIDIA ALCHEMIâ | 
| NVIDIA | DGX Spark HPC Developer Kit | 16 Nov | Compute (desktop) | Shipping | âOn display at SC26 and available for saleâ; HPC SDK playbook âcoming soonâ | 
| NVIDIA | Slurm quantum interface | 16 Nov | Scheduling | Announced | âSlurm 26.11 will be available later this monthâ | 
| NVIDIA | CUDA-Q QEC 0.9.0 with TCBP-JOBS decoder | 16 Nov | Quantum tools | Shipping | âAvailable â¦ todayâ | 
| NVIDIA, IBM, Fermilab | Quantum-classical codesign platform | 16 Nov | Quantum compute | Announced | âPlans for new collaborationsâ; no date | 
| OpenAI | Science blog | 15 Nov | Publishing | Shipping | Live | 
| OpenAI | ChatGPT for Academic Researchers expansion | 16 Nov | Access | Announced | Applications open 23 November | 
| OpenAI | GPT-6.2 Astra | 17 Nov | Model | Shipping | ChatGPT paid plans and API | 
| Alibaba | Qwen4-Max with science demonstrations | 16 Nov | Model | Shipping | QwenCloud and Alibaba Cloud; smaller open-weight Qwen4 models âsoonâ | 
| Anthropic | Claude Fable 5.2 with science results | 9 Nov | Model | Shipping | Claude API and major clouds | 
| Anthropic | âHow scientists use Claudeâ report | 5 Nov | Evidence | Published | Report | 
| Moonshot AI and DP Technology | Kimi K3.1 Pro in the BioMaster research agent | 12 Nov (SCMP: âon Thursdayâ) | Model in a domain platform | Announced | Collaboration announced; no terms reported | 

## NVIDIAâs hardware: built for simulation, arriving later

**Rubin HPC.** NVIDIAâs blog describes Rubin HPC as âa new GPU in the NVIDIA Vera Rubin platform designed to accelerate simulation and data processing for science, engineering and analytics workflows.â It has âmore than 50,000â CUDA cores, which NVIDIA calls a record for a single GPU, a sparse-data acceleration capability and seventh-generation NVLink. In his keynote, as NVIDIA summarised it, Buck said the chip supports precisions from FP64 to FP4 and has âabout three times as manyâ CUDA cores as Hopper.

The design argument is about memory, not raw arithmetic. NVIDIA says scientific workloads âtake a similar shape: an application reaches into an immense dataset, gathers a handful of scattered values, calculates a result and repeats this on an enormous scale.â Rubin HPC is meant to scale âdata movement and compute together rather than increasing compute alone,â which NVIDIA says helps sparse matrix operations, stencil computations, sorting and reductions. NVIDIA published no performance figures, memory capacity, power or price. The chip is âexpected to be available in 2028.â

**Vera CPU platform.** The Vera announcement is nearer and more detailed. NVIDIAâs release describes a rack of 192 Vera CPUs connected by Quantum-X800 InfiniBand, with âapproximately 300TB of memory and 1 petabyte of storageâ and more than 200TB/s of aggregate peak memory bandwidth, in a liquid-cooled design that reuses NVL72 rack components. Each Vera CPU has 88 custom Olympus cores, 176 threads and SOCAMM2 LPDDR5X memory âdelivering up to 1.2TB/s of bandwidth.â Each 1U tray holds eight CPUs.

NVIDIA reports two comparisons with x86 systems. âIn NVIDIAâs tests, Vera delivered 1.9x the performance and 2.4x the performance per wattâ on OpenFOAMâs motorBike Large airflow simulation. On the Weather Research and Forecasting model it reports âprojected performance gains of 1.6x and energy-efficiency gains of 2x.â The second figure is a projection, not a measurement, and the release does not name the x86 systems compared; it points to a separate performance brief for configurations.

The agent connection is Buckâs. NVIDIAâs keynote summary quotes him saying agents need âa CPU that can run, evaluate, run the tool, perform the operations, gather the results.â The one named customer is DKRZ, the German Climate Computing Center, which has selected Vera CPUs, alongside GB200 Grace Blackwell systems and Quantum-2 InfiniBand, for its HLRE-5 supercomputer. Bull is building it, and it is âexpected to be available in early 2028.â Buck said it joins Vera systems already announced at the Texas Advanced Computing Center.

**DGX Spark HPC Developer Kit.** The one piece of hardware on sale now is the smallest. The kit is a four-node cluster of DGX Spark systems connected in a ring over ConnectX-7 networking, with no switch. NVIDIA says four nodes give 512GB of combined memory and 20TB of NVMe storage, enough to run models of up to 700 billion parameters locally in NVFP4. Its stated purpose is to let researchers develop and debug multi-node work at a desk rather than wait in a cluster queue, and to keep unpublished results on premises. The HPC SDK playbook for it is âcoming soon.â

## Nemotron for Science: an open model packaged for labs

Nemotron for Science is the announcement most directly about agents. NVIDIA describes it as âscientific applications of its NVIDIA Nemotronâ¢ 3.5 Super VL open model, a new open chemistry dataset and post-training recipes to help researchers build specialized agents for scientific discovery.â

The model itself was not new at SC26. Hugging Faceâs records list the Nemotron 3.5 Super VL 120B-A12B repositories as created on 11 and 12 October 2026, with the BF16 model under the OpenMDW-1.1 licence. The new elements are the packaging, the recipes and the dataset, although the datasetâs Hugging Face record also dates from 12 October. LitmusBench v0.2, which NVIDIA describes as ânearly 50,000 questions for training and evaluating language models on chemistry reasoning,â is on Hugging Face as Nemotron-RL Litmus-Bench v0.2. Its card lists 47,118 training questions and 2,056 test questions in short-answer format, built from the ChEMBL, ExpansionRx and Biogen datasets and released for reinforcement-learning training. The card lists CC-BY-SA-4.0 and OpenMDW-1.1 licences.

NVIDIAâs release reports results from partners. Each is the partnerâs or NVIDIAâs claim, not an independent evaluation.

| Organisation | What it did with Nemotron | Reported result | 
|---|---|---|
| Terray Therapeutics | Post-trained it to extract reactants, products and conditions from reaction diagrams | Recovered âabout 76% of annotated reactionsâ on the RxnScribe test | 
| Lila Sciences | Designed lab protocols for automated labs; post-trained on lab feedback | Quantum-dot synthesis accuracy âto 61%, exceeding GPT-6 Astraâs 54% at 10-20x lower inference costâ | 
| Therna | Post-trained for mRNA therapeutic design | âNearly doubling accuracyâ in predicting protein-expression ranges; scientists preferred its explanations in 96% of comparisons with the base model | 
| KTH Royal Institute of Technology | Built an agent that repairs broken GROMACS setups | Fixed 81% of test cases in one overnight run on a single DGX Spark, each fix checked by GROMACS | 
| Argonne National Laboratory | Integrating it into the StructBioReasoner multi-agent system | Buck said an agent identified two new binding sites from a single prompt | 
| RIKEN | Made Nemotron models available on its RIKYU platform through the RiVault inference gateway | Availability, no result reported | 
| JÃ¼lich Supercomputing Centre | Integrating it into discovery agents | No result reported | 

Buckâs framing of the open-model choice was ownership: âYou own your data. You own the model.â That is the distinction NVIDIA is drawing with the closed frontier labs, whose models scientists reach through an API. The Lila comparison makes the trade explicit: a post-trained open model beating a frontier model on one narrow task at much lower cost. One narrow task, reported by the customer and the vendor, is a thin basis for a general conclusion, and the release does not give the size of the test set.

## CUDA-X: the tools an agent calls

If the model is the agentâs reasoning, the libraries are what it does. NVIDIAâs CUDA-X post presents them as âtools to help researchers make discoveries faster,â and Buck said NVIDIA now offers more than 1,000 CUDA-X libraries. The SC26 additions span astronomy, fusion, chemistry, materials and biology. Every speed-up below is NVIDIAâs or a partnerâs figure.

| Library | Domain | Claim | Status as described | 
|---|---|---|---|
| cuPhoton.xBeam | Radio astronomy (Event Horizon Telescope) | Correlation that âonce took over a month now takes eight minutesâ | Described as in use | 
| cuPhoton.xLens | Fusion plasma simulation (Pacific Fusion) | In-situ analysis while simulations run; can be coupled to a local Nemotron agent | Described as in use | 
| cuPotential | Molecular simulation | Accelerates existing simulation software âby up to 3xâ | NVIDIA working with SchrÃ¶dinger, AMBER, NAMD, GROMACS, LAMMPS and OMSF communities | 
| Daviator | Materials (VASP) | Quantum-mechanical calculations âup to 22x faster and at up to 75% lower costâ | âComing to NVIDIA ALCHEMIâ | 
| DAQIRI | Electron microscopy (Lawrence Berkeley National Laboratory) | Moves analysis into the experiment | Described as in use | 
| cuSpa | Spatial biology (Memorial Sloan Kettering) | â900x speedup compared with using CPUsâ | âNewly open sourceâ | 
| rapids-singlecell | Single-cell analysis | Therna processed data 200x faster | âAvailable in a single, agent-ready containerâ | 
| GPU-accelerated HMMER | Protein annotation (Basecamp Research) | Benchmark searches âup to 84x fasterâ | Open source | 

The phrase âagent-readyâ recurs, and it describes packaging more than new science: a library an agent can call through a container or a clear interface is easier to put in a loop. NVIDIA also showed an agent of its own. According to NVIDIAâs keynote summary, Buck said its new Nsight AI Porting Agent ported a routine from the WRF weather model to GPUs, âspeeding it up more than 5x using about 150 million tokens.â That is the most concrete statement in the SC26 material of what an agent costs to run on a real porting job, and it is a single example.

## Quantum: a scheduler and a decoder

The quantum announcements are about plumbing. NVIDIA says nearly 30 supercomputing centres are deploying more than 50 quantum processors, and that until now integrating one meant âa new software challenge for every new QPU or supercomputing center.â The new Slurm release lets QPU builders âcreate a single interface to their hardware for every supercomputing center, through a simple file,â building on the QRMI and QDMI quantum-HPC interfaces. NVIDIA names Alice & Bob, IBM, IQM, Pasqal, Quantinuum and MQSC as adopters. âSlurm 26.11 will be available later this month,â the post says. The practical effect, if it works as described, is that a scientist or an agent could request a quantum processor through the same scheduler as CPUs and GPUs.

On error correction, NVIDIA says its new TCBP-JOBS decoder, in the CUDA-Q QEC 0.9.0 library available now, delivers âup to 13,000xâ faster decoding than existing leading algorithms, âcutting the computation time for researching new quantum computing designs from 12 weeks to just 45 minutes.â This is a research-workload claim. Buck separately described the goal of cutting real-time decoding latency from about 300 microseconds to 10, which is a different and harder target.

The IBM and Fermilab item is a plan. NVIDIA says IBM and NVIDIA âare colocating NVIDIA DGX B200 systemsâ with IBM quantum computers, including those powered by IBM Quantum Nighthawk, to support a âjointly developedâ modular codesign platform with Fermilab focused on error correction and high-energy and nuclear physics. Fermilabâs chief technology officer is quoted describing a âproposed platform.â No date is given. NVIDIA also highlighted IBM and RIKEN work, named a finalist for the 2026 ACM Gordon Bell Prize, that modelled protein-ligand complexes of up to 12,635 atoms by combining quantum sampling with GPU calculations.

## What ships and what is announced

Sorting NVIDIAâs week by availability gives a clear pattern.

| Available now, per NVIDIA | Due within weeks | Due in 2027 | Due in 2028 or undated | 
|---|---|---|---|
| Nemotron 3.5 Super VL and LitmusBench v0.2 | Slurm 26.11 (later in November) | Vera CPU racks from four OEMs (early 2027) | Rubin HPC (2028) | 
| HPC SDK 26.11; Fortran compiler beta | DGX Spark HPC SDK playbook (âcoming soonâ) |  | DKRZ HLRE-5 (early 2028) | 
| CUDA-Q QEC 0.9.0 with TCBP-JOBS |  |  | Daviator in ALCHEMI (undated) | 
| cuSpa, GPU HMMER, rapids-singlecell container |  |  | IBM and Fermilab codesign platform (undated) | 
| DGX Spark HPC Developer Kit |  |  |  | 

The software is the part an agent can use this year. The new chips arrive on supercomputer procurement timescales: a centre planning a system for 2028 is the customer for Rubin HPC, not a lab building an agent next month. That gap is not a criticism of the roadmap, since HPC hardware is always announced years ahead, but it matters for reading the âagents for scienceâ framing. The agents NVIDIA described at SC26 run on todayâs GPUs, a DGX Spark on a desk, or a national-lab cluster.

## OpenAI: blog, study, access and a model

OpenAIâs science activity in the same four days was about users and claims rather than infrastructure. On 15 November it launched a blog for science, promising research results, âmethods and practical guidanceâ including âwhere a method falls short,â and perspectives from researchers. On 16 November it published âScience at the Frontier,â a study of 580,186 scientific messages from 1,819 scientists in its academic researcher programme, and announced 50,000 new grants of free ChatGPT Plus, each covering a researcher and up to two collaborators, with applications opening on 23 November. Our reading of the Science at the Frontier data covers what the study can and cannot show.

On 17 November, OpenAIâs GPT-6.2 Astra launch post said âwe believe GPT-6.2 Astra is the best model in the world for scientists,â citing a new high on Terminal-Bench-Science 0.1, a benchmark of scientific research workflows in a terminal, and on the Vals Science Index. OpenAIâs existing specialised route for biology is GPT-Rosalind, which its 16 November post says requires approval through a trusted-access process. Our comparison of GPT-6.2 Astra, Qwen4-Max and Claude Fable 5.2 sets OpenAIâs benchmark claims against independent results.

## Alibaba: Qwen4-Max in the lab

Alibabaâs Qwen4-Max launch on 16 November was a general model release, but its long-running agent section was mostly science. The Qwen blog describes a 48-hour autonomous run on Deep Principleâs Mira platform in which âQwen4 used the integrated dry- and wet-lab environmentâ to design a data-centre coolant molecule, build âa candidate space of 2.7 million molecules,â direct an automated wet lab through synthesis and testing, and after âfour iterationsâ obtain âa new, experimentally validated coolant molecule.â In a second demonstration on DP Technologyâs PharmMaster platform, aimed at the NLRP3 inflammasome as a target relevant to Alzheimerâs disease, the blog says Qwen4-Max made 454 tool calls, completed 3,239 computational metric evaluations and designed 24 candidate molecules, one with an estimated binding affinity âapproximately 3.6 times that of the referenceâ by relative binding free-energy calculation.

These are vendor demonstrations, and the coolant result is the stronger claim because it ends in a physical sample. The drug-design result is computational: DP Technologyâs experts, the blog says, consider the molecule worth âfurther wet-lab validation.â Alibabaâs enterprise post of 17 November describes such results as âin the case shown.â On the one independent science benchmark in the table below, Qwen4-Max scored far lower than the US frontier models, which suggests that strong agent demonstrations on partner platforms and general scientific-terminal skill are not the same thing.

## Anthropic: results and a usage study

Anthropicâs Fable 5.2 launch on 9 November made the most specific science claims of the month. It says Fable 5.2 âsolved two open problems in theoretical physics,â both concerning black holes and holographic duality, with preprints posted. It says Claude Mythos 5.2, the same model with fewer safeguards, devised a method to predict whole adeno-associated virus capsid structures and âaccurately modeled 28 of the 34 it attempted.â It says 1,000 Fable 5.2 agents formalised Fermatâs Last Theorem in 30 hours, a claim Anthropic amended on 11 November to describe more accurately what the model was given. Anthropic reports Fable 5.2 at 74.6% on Terminal-Bench-Science.

Anthropicâs 5 November report, âHow scientists use Claude,â is the usage-side counterpart to OpenAIâs study. It analyses 207,049 research sessions and finds data analysis and writing dominate, while idea generation and data gathering are underused relative to the importance surveyed scientists assign them. It also reports that on Claude Science, Anthropicâs science product, idea generation makes up 8% of sessions, âroughly on par with the share of researcher time these tasks take.â

## Moonshot: a stated shift, a partner, no announcement

Moonshot AIâs move is the least documented of the group. The South China Morning Post reported on 16 November that Giacomo Ficari, Moonshotâs director of international, told a Web Summit keynote in Lisbon: âThe real excitement is â¦ science.â He said that âwhile finance and legal are [at] the same level of intelligence [as] coding, science is actually bringing the level of intelligence even higher,â and that âwe are focusing a lot on biology and chemistry.â The Post reported that DP Technology announced a collaboration the previous Thursday to integrate Kimi K3.1 Pro into its BioMaster research agent, and that in DP Technologyâs internal evaluation Kimi K3.1 Pro paired with its coding tool scored highest against âother undisclosed model combinations.â In one demonstration, the Post said, the system analysed gene-expression data from 3,696 mouse pancreatic cells and identified 100 candidate genes in about 90 minutes, against an estimated four hours of human work.

The Decoder, citing the Post, called this a pivot âfrom coding to science as benchmark gains flatten.â That framing goes slightly beyond the Postâs account, which paraphrases Ficari as saying coding was âapproaching saturation in terms of technological progress,â prompting Moonshot to âexploreâ scientific and pharmaceutical applications. Moonshot did not immediately respond to the Postâs request for comment, and The Frontier Wire did not find a formal Moonshot announcement. The Post also noted a paper co-authored by Moonshot founder Yang Zhilin and Andrew Yao introducing a 100-task benchmark for original mathematical research, on which frontier models produced verifiable results but none independently solved the open conjectures.

DP Technologyâs presence in both the Alibaba and Moonshot stories is worth noting. A domain platform that runs drug-design or bioinformatics agents can swap the model underneath. For the model labs, that makes partner platforms a distribution channel; for the platform, it makes model choice a procurement decision, which is the same position NVIDIA is courting with an open model the platform can post-train itself.

## An independent check on âscienceâ claims

Most claims above are self-reported. Artificial Analysis runs Terminal-Bench-Science 0.1 independently: 70 expert-written research workflow tasks across the life, physical, earth, mathematical and engineering sciences, each passing only if every test passes, run with the mini-swe-agent harness and reported as pass@1 averaged over three repeats. Its results, as retrieved on 18 November 2026, give one common yardstick for the models named this month.

| Model (configuration listed by Artificial Analysis) | Terminal-Bench-Science 0.1 | 
|---|---|
| GPT-6.2 Astra (Max) | 78.1% | 
| Claude Fable 5.2 (Max, default fallback) | 73.3% | 
| Gemini 4 Carbon (High) | 64.8% | 
| Kimi K3.1 Pro | 34.3% | 
| Qwen4 Max | 23.3% | 

The ordering broadly agrees with OpenAIâs and Anthropicâs own claims for their models on this benchmark, and shows a large gap to the two Chinese models whose labs emphasised science this month. One benchmark of 70 tasks run in a generic harness does not measure what a model does on a partnerâs specialised platform with domain tools, which is where Alibabaâs and Moonshotâs demonstrations ran. It does suggest that the domain-platform results should not be read as evidence of general scientific capability. Artificial Analysis also lists Nemotron 3.5 Super VL in its data for this benchmark; the value shown is zero, and its page does not make clear whether that is a score or a missing run, so it is not reported in the table.

## What remains unknown

- **Rubin HPC specifications.** NVIDIA has given a core count and a year, not memory, bandwidth, power, FP64 throughput or price.
- **Veraâs x86 baselines.** The OpenFOAM and WRF comparisons point to a performance brief; the release does not name the systems, and the WRF figure is a projection.
- **Partner results.** The Nemotron partner figures and the Qwen4-Max and BioMaster demonstrations are vendor or partner reports without published test sets in the materials read.
- **Moonshotâs plans.** No announcement, model, product or dataset has been published by Moonshot itself.
- **Agent costs.** The WRF porting exampleâs 150 million tokens is the only cost figure for a science agent in the SC26 material. None of the labs reports what their science demonstrations cost to run.

Evidence that would change this picture includes Rubin HPC specifications and benchmarks, a public evaluation of post-trained Nemotron models on held-out scientific tasks, and independent runs of the Qwen4-Max and Kimi demonstrations on the platforms where they were performed. Our map of evidence levels in AI for biology offers a way to grade such claims as they arrive: describing a computational lead is a different level of evidence from a validated sample in a lab.

## Sources

## Questions readers ask

## When will NVIDIA's Rubin HPC GPU be available?

NVIDIA says Rubin HPC is expected to be available in 2028. It announced the chip at SC26 on 16 November 2026 as a new GPU in the Vera Rubin platform with more than 50,000 CUDA cores, sparse-data acceleration and seventh-generation NVLink.

## What is Nemotron for Science?

A package around NVIDIA's open Nemotron 3.5 Super VL model: scientific applications of the model, a chemistry dataset called LitmusBench v0.2 with nearly 50,000 questions, and post-training recipes for life sciences and chemistry. NVIDIA says the model and dataset are available now on Hugging Face.

## Which SC26 announcements can researchers use today?

By NVIDIA's own availability statements: Nemotron 3.5 Super VL and LitmusBench v0.2, NVIDIA HPC SDK 26.11, the CUDA-Q QEC 0.9.0 library with the TCBP-JOBS decoder, the newly open-source cuSpa library, and the DGX Spark HPC Developer Kit. Slurm 26.11 is due later in November; Vera CPU racks in early 2027; Rubin HPC in 2028.

## Has Moonshot AI officially pivoted Kimi to science?

Moonshot's international director said at Web Summit that science is where the 'real excitement' is, and DP Technology announced it is integrating Kimi K3.1 Pro into its BioMaster research agent, according to the South China Morning Post. 'Pivot' is The Decoder's word. Moonshot did not immediately respond to the Post's request for comment, and The Frontier Wire found no formal Moonshot announcement.

## Did The Frontier Wire test any of these products?

No. This survey reads NVIDIA's SC26 press releases and blog posts, the OpenAI, Alibaba and Anthropic announcements, Hugging Face model records, Artificial Analysis evaluation data and press reports where no primary source was available. Performance figures are attributed to the company that reported them.
