---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-recommits-to-fp64-with-new-rubin-hpc-gpu-for-science
title: "NVIDIA Recommits to FP64 With New Rubin HPC GPU for Science"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: ["2026-11-16"]
keywords: ["nvidia", "fp64", "rubin", "data", "cores", "core", "simulation", "recommits"]
source: https://www.indiekings.com/2026/11/nvidia-rubin-hpc-gpu-fp64-sc26.html
source_anchor: ""
source_lines: [1, 106]
section: "NVIDIA_Hardware"
---
# NVIDIA Recommits to FP64 With a New GPU: The Rubin HPC

**NVIDIA has unveiled the Rubin HPC, a new GPU purpose-built for scientific simulation, engineering and large-scale data analysis, at the SC26 supercomputing conference in Chicago.** The chip packs more than 50,000 CUDA cores — the highest core count NVIDIA has ever put on a single GPU — along with sparse data acceleration and seventh-generation NVLink. According to HPCwire, the design carries three times the vector cores of Blackwell, which the outlet reads as NVIDIA recommitting to FP64 after two generations of stagnation in double-precision performance. The GPU is expected in 2028.

There's an important nuance to the FP64 story: NVIDIA's own announcement never uses the word "FP64" and discloses no teraflops figures. The recommitment interpretation comes from the design details and from NVIDIA's positioning of the chip for traditional high-performance computing. Here's what was announced, what wasn't, and why it matters for the future of scientific computing.

## What is the NVIDIA Rubin HPC GPU?

**The Rubin HPC is a new member of NVIDIA's Vera Rubin platform, but it is designed for simulation rather than AI model training.** While the standard Rubin GPU targets the AI datacenter market, the Rubin HPC is aimed at scientists, engineers and analysts working with large datasets — the classic modeling and simulation workloads that supercomputers were originally built for.

The design rests on three pillars:

- **A record number of CUDA cores:** more than 50,000 on a single GPU, up from 28,672 in the current Rubin GPU. This is the largest disclosed CUDA core count NVIDIA has ever shipped.
- **Sparse data acceleration:** hardware support for workloads that pick scattered values from huge archives — sparse matrices, stencil calculations, sorting and reductions — keeping the massive core count fed with data.
- **Seventh-generation NVLink:** the GPU-to-GPU interconnect that links multiple chips into a single computational domain.

NVIDIA frames the design philosophy as growing compute and data movement together. A simulation that repeats an operation billions of times is wasted silicon if the cores spend their cycles waiting for data — so sparse data acceleration is as central to the pitch as raw core count.

## Why does the Rubin HPC signal a renewed FP64 commitment?

**HPCwire's reading is that the Rubin HPC marks NVIDIA's return to double-precision computing after two GPU generations that barely moved the needle on FP64.** The evidence is architectural: according to the outlet, the chip carries three times the vector cores of Blackwell, and vector cores are what drive native FP64 throughput.

The context makes this significant. Double precision (FP64) — numbers written with 64 bits, giving about sixteen significant digits — is essential for scientific simulation. Weather models, aerodynamics calculations and financial risk simulations evolve a system one step at a time, with each step's output feeding the next. Tiny rounding errors compound across billions of steps until they distort the final result. AI workloads have the opposite profile: neural networks tolerate coarse numbers down to 8 or even 4 bits, which is why modern AI chips have concentrated nearly all their silicon on low precision.

NVIDIA's recent track record on FP64 tells the story of that shift:

| Generation | Vector FP64 (TFLOPS) | Tensor core FP64 (TFLOPS) | 
|---|---|---|
| Ampere A100 (2020) | 9.7 | 19.5 | 
| Hopper H100 and H200 | 34 | 67 | 
| Blackwell B100 (2024) | 30 | 30 | 
| Rubin (AI GPU) | 33 | not native (emulated: 200) | 

Blackwell actually regressed on tensor core FP64 compared to Hopper, and Jack Dongarra — University of Tennessee professor and one of the founders of the TOP500 list — called out the stagnation at SC25 in November 2025. For traditional simulations, the older, cheaper Hopper cards remained the best NVIDIA option. Against that backdrop, a simulation-focused chip with triple Blackwell's vector cores looks like a deliberate correction.

One honest caveat: **NVIDIA has not disclosed the Rubin HPC's actual FP64 teraflops**, nor its memory, bandwidth or power figures. HPCwire's estimate of roughly 100 TFLOPS rests on the assumption that performance scales with core count — a reasonable guess, but untested. Until NVIDIA publishes numbers, the "recommitment" is a direction, not a verified specification.

## How does NVIDIA's FP64 strategy compare to AMD's?

**The two chipmakers have taken opposite approaches to double precision, and the rivalry is sharpening.** NVIDIA's path runs through emulation: its Ozaki scheme chains multiple operations on low-precision tensor cores to achieve near-FP64 accuracy. By NVIDIA's own figures, the standard Rubin AI GPU reaches 200 TFLOPS of emulated FP64 on matrix operations — up from 150 on Blackwell and 67 on Hopper.

AMD chose the native route and says so loudly. Its Instinct MI430X, expected in 2027, claims **288 TFLOPS of native FP64** — nearly nine times the 33 native TFLOPS of the AI-oriented Rubin. AMD also argues that emulation doesn't cover every case and can return wrong answers in some situations.

NVIDIA's counterargument is specialization rather than one-size-fits-all silicon. Dion Harris, the company's senior director for HPC and AI infrastructure, told HPCwire that each job needs the right GPU, with a range of precisions spanning FP4 through FP32 and FP64. The Rubin HPC appears to follow exactly that logic: a dedicated part for workloads where native double precision matters, running alongside the AI-focused Rubin rather than replacing it.

There's also political pressure behind the scenes. Darío Gil, the U.S. Under Secretary of Energy for Science, told HPCwire in February that he had received commitments from both Jensen Huang and AMD's Lisa Su to keep FP64 alive. For the Department of Energy, AI doesn't replace high-precision simulation — it depends on it, using simulation to generate and validate the training data for surrogate models.

## Who is the Rubin HPC actually for?

**The target customers are national laboratories, automakers, aircraft manufacturers, pharmaceutical companies and financial firms — essentially, anyone who simulates before building anything.** Ian Buck, NVIDIA's vice president of hyperscale and HPC, ties the chip directly to the rise of AI agents: if an agent is going to try dozens of candidate materials or designs, the simulation that validates each candidate has to be fast. The Rubin HPC is positioned as that validation engine.

For everyone else — including the AI market that dominates the headlines — nothing changes. The GPUs facing shortages and benchmark races remain the AI chips. The Rubin HPC speaks to a different buyer with different needs, and its arrival in 2028 means it won't affect any purchasing decision for more than a year.

## How does the Rubin HPC fit into the current supercomputing landscape?

**The announcement lands against a TOP500 list that is notably standing still.** The November 2026 rankings, released the same day, show an identical top ten to June, led by LineShine from China's national center in Shenzhen at 2.198 exaflops, ahead of El Capitan (1.809) and Frontier (1.353). New systems number just 27 — 5.4 percent of the list, the lowest turnover in TOP500 history — and the highest new entry, South Korea's HANGANG at KISTI, sits at number twelve.

The contrast is striking: the machines leading the rankings still run on current or previous generations, while the chip meant to put 64-bit computing back in the spotlight won't arrive until 2028. In the meantime, laboratories that need native FP64 face an awkward menu — aging Hopper cards, Blackwell with emulation overhead, or AMD's competing silicon.

The competitive timeline matters too. AMD's MI430X is due in 2027, a year before the Rubin HPC, and no independent benchmark has yet compared the two approaches on the same scientific codes. NVIDIA's 2028 window means AMD gets a head start with the native-FP64 audience — and by the time Rubin HPC ships, its real-world performance will face immediate comparison.

## What are the open questions about the Rubin HPC?

**Three gaps in the announcement will determine whether the FP64 recommitment is real in practice.**

- **No FP64 numbers.** Without disclosed teraflops, there's no telling whether the Rubin HPC beats even the aging Hopper on double precision. The ~100 TFLOPS estimate circulating is proportional extrapolation, not data.
- **No memory or bandwidth figures.** Scientific workloads are frequently memory-bound, and HBM capacity and bandwidth often matter more than raw compute. Their absence makes performance projections impossible.
- **No pricing.** Cost per simulation is what laboratories actually budget for, and NVIDIA has said nothing about it.

All of these can change before 2028 — the specifications were clearly announced early, and NVIDIA revised Vera Rubin's memory figures as recently as October. The chip's real test arrives when independent benchmarks run it against AMD's native approach on production scientific codes.

## Frequently Asked Questions

### What is the NVIDIA Rubin HPC?

The Rubin HPC is a new GPU NVIDIA unveiled at SC26 on November 16, 2026. Designed for scientific simulation, engineering and data analytics rather than AI training, it features more than 50,000 CUDA cores, sparse data acceleration and seventh-generation NVLink, with availability expected in 2028.

### Is the Rubin HPC the same as the AI-focused Rubin GPU?

No. The Rubin HPC is a separate, newly announced GPU dedicated to simulation and data analysis. It sits alongside the standard Rubin GPU, which targets AI datacenters and offers 33 TFLOPS of native vector FP64 with 200 TFLOPS of emulated matrix performance.

### Has NVIDIA disclosed the Rubin HPC's FP64 performance?

No. NVIDIA's announcement does not mention FP64 or provide any teraflops figures. HPCwire's interpretation of a renewed FP64 commitment is based on the chip reportedly carrying three times the vector cores of Blackwell; an estimate of roughly 100 TFLOPS circulating in coverage is unverified extrapolation.

### When will the Rubin HPC be available?

NVIDIA expects the Rubin HPC in 2028. Pricing, memory specifications, bandwidth and power figures have not been disclosed, and AMD's competing MI430X is due in 2027, a year earlier.

### How does NVIDIA's FP64 approach differ from AMD's?

NVIDIA pursues FP64 through Ozaki-style emulation on low-precision tensor cores (200 TFLOPS emulated on the standard Rubin AI GPU), plus now a dedicated simulation chip. AMD's MI430X targets 288 TFLOPS of native FP64 and claims emulation can produce wrong answers in some cases — a warning that comes from the competitor, not an independent source.

### Why does scientific computing need FP64?

Simulations repeat calculations across billions of steps, each one building on the last, so small rounding errors compound. Double precision provides about sixteen significant digits to keep errors bounded. AI models, by contrast, tolerate much lower precision, which is why AI hardware has shifted away from FP64 silicon.

## Key takeaways

- **NVIDIA unveiled the Rubin HPC at SC26 on November 16, 2026,** a simulation-focused GPU with more than 50,000 CUDA cores, sparse data acceleration and seventh-generation NVLink, expected in 2028.
- **HPCwire reads the design as a recommitment to FP64,** citing three times Blackwell's vector cores — but NVIDIA disclosed no FP64 teraflops, memory or bandwidth figures.
- **Native FP64 has been flat across NVIDIA's recent generations,** with Blackwell actually regressing on tensor core FP64 versus Hopper, and the AI-oriented Rubin relying on Ozaki emulation for matrix double precision.
- **AMD is pursuing the opposite strategy** with the MI430X's claimed 288 TFLOPS of native FP64, due in 2027 — a year before the Rubin HPC.
- **Everything definitive arrives in 2028:** the chip's real FP64 numbers, its pricing, and the first independent head-to-head benchmarks against AMD's native approach.

The Rubin HPC is NVIDIA's answer to a genuine tension in modern computing: the AI boom pulled silicon away from the double precision that science depends on, and the laboratories noticed. A dedicated simulation GPU with triple Blackwell's vector cores suggests NVIDIA wants to serve both worlds — but with no disclosed FP64 numbers, memory specs or pricing, the announcement is a promise rather than a proof. The verdict arrives in 2028, when benchmark results either validate the recommitment or hand the argument to AMD's native approach. If this breakdown helped you understand the announcement, share it with colleagues following the SC26 news — and watch for NVIDIA's first specification disclosures, which will tell us far more than any keynote slide.
