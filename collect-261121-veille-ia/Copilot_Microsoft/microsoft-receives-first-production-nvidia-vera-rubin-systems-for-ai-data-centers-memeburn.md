---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-receives-first-production-nvidia-vera-rubin-systems-for-ai-data-centers-memeburn
title: "Microsoft Receives First Production NVIDIA Vera Rubin Systems for AI Data Centers - Memeburn"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["vera", "rubin", "production", "nvidia", "microsoft", "first", "million", "nvl72"]
source: https://memeburn.com/microsoft-receives-first-production-nvidia-vera-rubin-systems-for-ai-data-centers/
source_anchor: ""
source_lines: [1, 42]
section: "Copilot_Microsoft"
---
TL;DR

- Microsoft CEO Satya Nadella confirmed the first production NVIDIA Vera Rubin NVL72 racks have arrived at Microsoft data centers
- This follows Microsoft's earlier claim as the first hyperscaler to validate a Vera Rubin system in a lab setting back in March 2026
- Each Vera Rubin NVL72 rack contains 72 Rubin GPUs and 36 Vera CPUs, delivers 3.6 exaflops of inference performance, and costs between $7 million and $8 million
- Industry analyst Ben Bajarin noted that rack-scale compute is still less than 10% of the total accelerator installed base, a figure he expects to grow to 35-40% within a few years
- The delivery comes days before NVIDIA's quarterly earnings on August 27, where investors will look for early Vera Rubin revenue signals

The first production NVIDIA Vera Rubin systems have arrived at Microsoft’s data centers. CEO Satya Nadella posted photos on X showing the rack-scale AI platforms being unloaded at what he described as a delivery day for Microsoft’s infrastructure teams, thanking both NVIDIA and the Azure hardware group for reaching the milestone.

This is not a lab exercise. Microsoft validated a Vera Rubin NVL72 system in a controlled environment back in March, claiming the title of first hyperscaler to power one on. What Nadella is announcing now is different: production hardware going into production facilities, the step that turns a technology demonstration into deployable capacity.

The timing is pointed. NVIDIA reports earnings on August 27, five days from now, and investors have been waiting to hear whether Vera Rubin revenue would start appearing in the company’s numbers. NVIDIA’s forward guidance and Vera Rubin’s ramp schedule are central to the bull-bear debate heading into the report. A public delivery confirmation from NVIDIA’s largest cloud customer is not an accident of scheduling.

## **What is actually arriving**

Each Vera Rubin NVL72 rack is a single AI supercomputer contained in a cabinet. Inside: 72 Rubin GPUs and 36 Vera CPUs, connected by sixth-generation NVLink with 260 TB/s of aggregate bandwidth. The system delivers 3.6 exaflops of FP4 inference performance and 75 TB of fast memory. It requires 100% liquid cooling and an 800-volt DC power architecture. It cannot run in air-cooled facilities. It weighs roughly 4,000 pounds. Each rack costs between $7 million and $8 million.

NVIDIA designed the Vera CPU from scratch for this generation, using 88 custom Olympus cores rather than licensing ARM’s Neoverse designs as it did with the Grace CPU in Blackwell. The entire platform is built on TSMC’s 3nm process, with HBM4 memory supplied primarily by SK Hynix.

Production did not arrive without complications. A thermal lid manufacturing problem pushed back initial shipments by several weeks in July, compounded by slower HBM4 memory qualification. KeyBanc still projected 1.7 to 1.8 million Rubin units for 2026 and raised its NVIDIA price target despite the setback. The delivery to Microsoft suggests the production line has recovered.

The performance gains over the previous generation are substantial. CoreWeave, one of the first partners to run benchmarks on production Vera Rubin hardware, measured up to 10x more tokens per megawatt compared to the GB200 Blackwell NVL72 when running DeepSeek R1. NVIDIA’s own figures claim the platform delivers AI training with one-fourth the GPUs and AI inference at one-tenth the cost per million tokens compared to Blackwell.


## **Reactions from industry observers**

The community response to Nadella’s post was notably forward-looking. Nicolas Bustamante, a tech investor, called Vera Rubin “an absolute beast,” citing roughly 3x better AI compute than the GB300 and 2.7x the memory bandwidth, with significantly more tokens per megawatt. He expects the performance improvements to drive a visible acceleration in AI capability.

But it was analyst Ben Bajarin who offered perhaps the more consequential framing. Based on his firm’s accelerator installed base model, rack-scale compute **still represents less than 10% of the total accelerator installed base**. Bajarin projected that when that share crosses 35 to 40%, “it will be a very different world.”

That observation is worth sitting with. The AI infrastructure buildout that has already consumed hundreds of billions in capital spending across Microsoft, Google, Amazon, and Meta is still operating on hardware that will look outdated within two to three years. The transition to rack-scale systems like Vera Rubin is not a refresh. It is a replacement cycle that could dwarf the initial deployment wave.

## **Where this fits in the bigger buildout**

Microsoft’s Fairwater AI superfactory in Mount Pleasant, Wisconsin went live ahead of schedule in April with clusters of GB300 Blackwell Ultra racks. The facility was engineered from the start to accommodate future generations, including Vera Rubin’s higher power and cooling demands. Atlanta is the other confirmed Fairwater site. The data center design philosophy Nadella has articulated publicly is flexibility: avoid locking hundreds of megawatts into a single chip generation, because the power per rack and cooling requirements change dramatically between Blackwell, Vera Rubin, and the 2027 Rubin Ultra (codenamed Kyber).

Microsoft is not alone in receiving production Vera Rubin hardware. NVIDIA has confirmed that AWS, Google Cloud, Oracle Cloud, CoreWeave, Lambda, Nebius, and Nscale are all in the first deployment cohort for the second half of 2026. OpenAI has reportedly committed to large-scale Vera Rubin deployment starting in Q3. Beyond the cloud providers, Bristol Myers Squibb purchased an eight-rack DGX SuperPOD powered by Vera Rubin for drug discovery work, signaling that enterprise buyers are beginning to procure this hardware directly.

AMD is also now competing at rack scale with its Helios platform, offering more memory per accelerator than Vera Rubin at the expense of NVIDIA’s inference throughput and its entrenched CUDA software ecosystem. Whether that competitive pressure translates into actual hyperscaler procurement shifts remains to be seen, but NVIDIA’s response has been to compress its product cycle further: Rubin Ultra is already scheduled for 2027, only a year after the platform now being delivered.

For now, the image is simple enough. A CEO posted photos of enormous server racks being wheeled into a data center. Behind that image is a $7 million-per-rack hardware transition that is still in its earliest phase, and an industry that is only beginning to understand how much more infrastructure the next generation of AI will demand.
