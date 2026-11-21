---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-vera-rubin-rubin-gpu-vera-cpu-and-nvl72-full-guide
title: "NVIDIA Vera Rubin: Rubin GPU, Vera CPU and NVL72 Full Guide"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["rubin", "vera", "nvidia", "nvl72", "production", "rack", "full", "racks"]
source: https://www.insidedeeptech.com/nvidia-vera-rubin-nvl72-full-guide/
source_anchor: ""
source_lines: [1, 266]
section: "NVIDIA_Hardware"
---
Blackwell taught the industry to buy AI compute by the rack. NVIDIA Vera Rubin is the first generation where the GPU, the CPU, the memory, the rack and the network were all redesigned at once, and as of November 16, 2026 it's no longer a roadmap slide: NVIDIA's Vera Rubin NVL72 began production shipments in August 2026, per the Q2 fiscal 2027 earnings call, and it is now generally available on Oracle Cloud Infrastructure (NVIDIA).

So the useful question isn't what Rubin is. It's what Rubin changes. The short answer is memory and plumbing: per NVIDIA's Rubin GPU architecture deep dive, each GPU carries up to **288 GB** of HBM4 at up to **22 TB/s**, a **2.8x** jump over Blackwell, and the Vera Rubin NVL72 rack ties **72** of those GPUs to **36** Vera CPUs over NVLink 6.

This full guide covers the Rubin GPU, the Vera CPU, the NVL72 rack (and why it used to be called NVL144), HBM4, networking, the Rubin Ultra and Kyber roadmap, and the honest limits. If you're still deploying the previous generation, start with Inside Deep Tech's GB200 NVL72 full guide.

Last updated: November 16, 2026.

## Key Takeaways

- Vera Rubin NVL72 pairs 72 Rubin GPUs with 36 Vera CPUs (NVIDIA); shipments began in August 2026, and CoreWeave, OCI and Google Cloud now offer it (NVIDIA).
- Each Rubin GPU lists 288 GB HBM4 and 50 PFLOPS NVFP4 (sparse); bandwidth is quoted at 19.2 TB/s or up to 22 TB/s.
- NVLink 6 doubles per-GPU scale-up to 3.6 TB/s; NVIDIA's spec table lists 216 TB/s per rack, its POD blog 260 TB/s.
- "NVL144" and "NVL72" name the same 36-CPU rack class: 2025 materials counted 144 GPUs, 2026 materials count dual-die packages.
- Honest limits: supply stays a bottleneck through fiscal 2028, no public rack power figure, and Rubin CPX's status is unconfirmed.

## What NVIDIA Vera Rubin actually is in November 2026

Vera Rubin is a platform name, not a chip. NVIDIA's GTC 2026 announcement on March 16, 2026 defined it as seven chips in full production: the Vera CPU, Rubin GPU, NVLink 6 Switch, ConnectX-9 SuperNIC, BlueField-4 DPU, Spectrum-6 Ethernet switch, and the newly integrated Groq 3 LPU.

Those chips ship in five rack types, per the same GTC 2026 release: Vera Rubin NVL72 GPU racks, Vera CPU racks, Groq 3 LPX inference racks, BlueField-4 STX storage racks, and Spectrum-6 SPX Ethernet racks. NVIDIA's Vera Rubin POD blog describes the reference POD as **40** racks with **1,152** Rubin GPUs and **60** exaflops.

NVIDIA's CES 2026 launch release also introduced the HGX Rubin NVL8 system for eight-GPU servers. Vera is the CPU. Rubin is the GPU. The NVL number is the size of the NVLink domain.

### Timeline: from roadmap slide to production shipments

| Date | Milestone | Source | 
|---|---|---|
| Sep 9, 2025 | Rubin CPX and the Vera Rubin NVL144 CPX rack announced; CPX targeted for end of 2026 | NVIDIA Newsroom | 
| Sep 12, 2025 | SK hynix completes HBM4 development and readies mass production | SK hynix | 
| Jan 5, 2026 | Rubin platform launched at CES with six chips | NVIDIA Newsroom | 
| Feb 12, 2026 | Samsung begins HBM4 mass production and ships commercial parts | Samsung | 
| Mar 16, 2026 | GTC: seven chips in full production; Micron HBM4 in high-volume production | NVIDIA, Micron | 
| May 31, 2026 | Full production ramp; shipments set to begin "this fall" | NVIDIA Newsroom | 
| Aug 2026 | Production shipments of Vera Rubin commence | Q2 FY27 call | 

Source: NVIDIA Newsroom, NVIDIA Q2 fiscal 2027 earnings call transcript, and the supplier releases linked in the table.

Read those dates carefully. "Full production" in March meant chips. Production shipments of systems began in August, and NVIDIA's CFO guided that Vera Rubin would be about **20%** of data center revenue in the fiscal third quarter, per the same call.

### Since August: who is actually running Vera Rubin NVL72

Shipping and renting are different milestones. Supermicro said on September 23, 2026 that it is shipping Vera Rubin NVL72 racks integrated with its direct liquid cooling stack (Supermicro). CoreWeave announced availability on September 30, naming Cognition as the first customer anywhere running production workloads on the system, with up to 4.8x higher total token throughput on SWE-2 inference versus a GB200 NVL72 baseline (CoreWeave). That is a customer-reported number on one workload, not a general benchmark.

NVIDIA and Oracle said on October 27 that Vera Rubin NVL72 and Vera CPUs are now generally available on OCI, with plans to expand OCI Superclusters to multi-gigawatt scale and to add HGX Rubin NVL8 servers (NVIDIA Newsroom). Google Cloud said on October 21 that A5X instances built on Vera Rubin NVL72 would reach general availability the following month, and that it plans to scale to as many as 960,000 Rubin GPUs across multiple sites (NVIDIA blog).

The next hard number arrives on November 17, 2026, when NVIDIA reports the quarter that ended October 25 (NVIDIA IR). On the August call, the CFO guided Vera Rubin to about 20% of data center revenue in that quarter (Q2 FY27 transcript). Treat the actual mix as the first public read on how fast racks are leaving the factory.

| Date (2026) | Milestone | Source | 
|---|---|---|
| Aug | Production shipments of Vera Rubin begin | NVIDIA Q2 FY27 call | 
| Sep 16 | First MLPerf Inference results (v6.1, preview category) | NVIDIA | 
| Sep 23 | Supermicro says it is shipping Vera Rubin NVL72 racks | Supermicro | 
| Sep 30 | CoreWeave availability; Cognition first production customer | CoreWeave | 
| Oct 21 | Google Cloud A5X announced, general availability the following month | NVIDIA blog | 
| Oct 27 | Vera Rubin NVL72 and Vera CPUs generally available on OCI | NVIDIA Newsroom | 
| Nov 17 | NVIDIA reports the quarter ended October 25 | NVIDIA IR | 

Source: NVIDIA Q2 FY27 call, Supermicro, CoreWeave, NVIDIA Newsroom, NVIDIA blog, NVIDIA IR.

## The Rubin GPU: two reticle dies, HBM4, and a new Transformer Engine

Per NVIDIA's Rubin GPU architecture blog, published July 21, 2026, Rubin is two reticle-limited compute dies joined on one package by the NVIDIA High-Bandwidth Interface (NV-HBI). The package carries **336 billion** transistors, **224** streaming multiprocessors and **896** Tensor Cores.

The headline figure is up to **50 petaflops** of NVFP4 inference per GPU, which NVIDIA's product spec table footnotes as a sparse number. The dense figures are the ones to plan around: **35 PFLOPS** NVFP4 training, **17.5 PFLOPS** FP8/FP6 and **4 PFLOPS** FP16/BF16 per GPU.

The architecture changes matter more than the peak. The same deep dive lists:

- Tensor Cores that process twice the data along the K dimension per clock (NVIDIA).
- A 3-bit lookup-table weight format that NVIDIA says can retain up to MXFP8 accuracy.
- 2:4 activation sparsity for attention, plus 2x FP32 and 4x BF16/FP16 exponential throughput versus Blackwell for softmax.
- Counted writes that cut synchronization overhead on device-initiated NVLink transfers.

Per-GPU links, from the Rubin architecture blog: NVLink 6 at **3,600 GB/s**, NVLink-C2C to the Vera CPU at **1,800 GB/s**, and x16 PCIe Gen 6 at up to **256 GB/s**.

### Rubin GPU, Vera Rubin Superchip, and NVL72 rack specs

| Metric | Rubin GPU | Vera Rubin Superchip | Vera Rubin NVL72 | 
|---|---|---|---|
| Configuration | 1 Rubin GPU | 2 Rubin GPUs + 1 Vera CPU | 72 Rubin GPUs + 36 Vera CPUs | 
| NVFP4 inference (sparse) | 50 PFLOPS | 100 PFLOPS | 3,600 PFLOPS | 
| NVFP4 training (dense) | 35 PFLOPS | 70 PFLOPS | 2,520 PFLOPS | 
| FP8/FP6 training (dense) | 17.5 PFLOPS | 35 PFLOPS | 1,260 PFLOPS | 
| FP16/BF16 (dense) | 4 PFLOPS | 8 PFLOPS | 288 PFLOPS | 
| FP64 | 33 TFLOPS | 67 TFLOPS | 2,400 TFLOPS | 
| HBM4 capacity / bandwidth | 288 GB / 19.2 TB/s | 576 GB / 38.5 TB/s | 20.7 TB / 1,400 TB/s | 
| NVLink bandwidth | 3 TB/s | 6 TB/s | 216 TB/s | 
| CPU memory | n/a | Up to 1.5 TB LPDDR5X | Up to 54 TB LPDDR5X | 
| CPU cores | n/a | 88 Olympus cores, 176 threads | 3,168 Olympus cores | 
| Scale-out networking (bidirectional) | 0.45 TB/s | 0.9 TB/s | 32.4 TB/s | 

Source: NVIDIA Vera Rubin NVL72 product page, spec table (sparse and dense footnotes as published), retrieved October 5, 2026.

Notice the per-GPU NVLink figure. The spec table says **3 TB/s**, while NVIDIA's platform deep dive and CES release say **3.6 TB/s**. The conflicting-numbers section below explains how to handle that.

## The Vera CPU: 88 Olympus cores built for agent sandboxes

Vera succeeds Grace as NVIDIA's data center CPU. NVIDIA's Vera CPU architecture blog describes **88** custom Olympus cores with full Arm compatibility, Spatial Multithreading that partitions core resources between two hardware threads, a **164 MB** unified L3 cache, and a Scalable Coherency Fabric with up to **3.4 TB/s** of bisection bandwidth.

Memory is SOCAMM2 LPDDR5X with up to **1.2 TB/s** of aggregate bandwidth in field-replaceable modules, per the same blog. NVIDIA's spec table lists up to **1.5 TB** of LPDDR5X per Vera CPU, and Micron says its SOCAMM2 enables up to 2TB and 1.2 TB/s per CPU.

Why does a CPU matter this much? Agentic workloads push tool calls, code execution and reinforcement learning environments onto CPU cores. The GTC 2026 release introduced a standalone Vera CPU rack with **256** liquid-cooled Vera CPUs, and NVIDIA's POD blog says one rack can sustain over **22,500** concurrent RL or agent sandbox environments.

Inside Deep Tech's take: the Vera CPU rack is this generation's sleeper product. It turns NVIDIA's CPU from a GPU companion into a product line aimed straight at x86 sandbox fleets.

## Inside the Vera Rubin NVL72 rack

The rack keeps the NVL72 footprint but rebuilds almost everything inside it. Per NVIDIA's Vera Rubin POD blog, it holds **18** compute trays and **9** NVLink switch trays, about **1.3 million** components, and weighs roughly **4,000** lbs. Each compute tray carries two Vera Rubin superchips.

Trays are cable-free, hose-free and fanless. NVIDIA says tray assembly drops from nearly two hours to about five minutes, and the NVLink spine at the back uses four cable cartridges holding **5,000** copper cables (POD blog).

Power and cooling changed too. The same POD blog lists a **45°C** warm-water inlet, liquid-cooled busbars rated up to **5,000 A**, and Intelligent Power Smoothing with **6x** more rack-level energy storage (**400 J** per GPU) that cuts peak current demand by up to **25%**.

That 45°C figure matters for facilities. NVIDIA says a 45°C inlet lets many climates use dry coolers instead of chillers, freeing enough power for up to **10%** more NVL72 racks in the same budget. For cold plates and CDUs, the Blackwell-era rules in Inside Deep Tech's GB200 NVL72 guide still apply.

## Why NVL144 became NVL72, and why the numbers don't all match

If you followed the 2025 roadmap, you remember "Vera Rubin NVL144." NVIDIA's Rubin CPX announcement of September 9, 2025 described a Vera Rubin NVL144 CPX rack with **144** Rubin GPUs, **144** Rubin CPX GPUs and **36** Vera CPUs.

The 2026 materials describe the same CPU count with **72** Rubin GPUs (NVIDIA), and each Rubin package contains two reticle-sized dies (architecture blog). Inside Deep Tech reads this as a counting change, dies in 2025 and packages in 2026, rather than a smaller rack. NVIDIA hasn't published a formal explanation of the rename in the sources reviewed for this guide.

The spec mismatches are more practical. Here's where NVIDIA's own pages disagree:

| Metric | Product spec table | Developer blogs and launch material | 
|---|---|---|
| Per-GPU HBM4 bandwidth | 19.2 TB/s | Up to 22 TB/s | 
| Rack HBM4 bandwidth | 1,400 TB/s | 1.6 PB/s | 
| Per-GPU NVLink bandwidth | 3 TB/s | 3.6 TB/s | 
| Rack NVLink bandwidth | 216 TB/s | 260 TB/s | 

Source: NVIDIA product page, Rubin GPU architecture blog, Vera Rubin platform blog, Vera Rubin POD blog, scale-up blog.

## HBM4: the memory that sets Rubin's ship rate

Rubin moves NVIDIA's data center GPUs to HBM4, which doubles the interface to **2,048** I/O terminals per stack (SK hynix). All three DRAM makers have announced HBM4 production:

| Supplier | Milestone | Published HBM4 spec | 
|---|---|---|
| SK hynix | Development complete, mass production readied (Sep 12, 2025) | 2,048 I/O, above 10 Gbps, over 40% better power efficiency | 
| Samsung | Mass production and commercial shipments (Feb 12, 2026) | 11.7 Gbps (up to 13 Gbps), up to 3.3 TB/s per stack, 24 to 36 GB at 12-high | 
| Micron | High-volume production for Vera Rubin (Mar 16, 2026) | 36GB 12H, over 11 Gb/s, above 2.8 TB/s, 2.3x its HBM3E bandwidth | 

Source: SK hynix Newsroom, Samsung Semiconductor Newsroom, Micron investor release.

NVIDIA's **288 GB** per GPU lines up with eight 36 GB 12-high stacks. That's Inside Deep Tech's arithmetic, not an NVIDIA disclosure; the architecture blog confirms only 12-Hi stacks and the total.

The bigger issue is price. On the Q2 fiscal 2027 call, NVIDIA's CFO said the company is "experiencing extreme pricing conditions in memory," and inventory rose to **$32 billion** as NVIDIA prepared for the Vera Rubin launch.

For how HBM stacks, base dies and packaging slots gate GPU supply, see Inside Deep Tech's HBM full guide.

## Networking: NVLink 6, ConnectX-9, and co-packaged optics

Scale-up stays on copper inside the rack. NVLink 6 delivers **3.6 TB/s** per GPU across an all-to-all **72**-GPU topology, with in-network compute for collectives, per NVIDIA's CES release.

Scale-out moves to ConnectX-9 SuperNICs at **1.6 Tb/s** per GPU, and the switch tier moves to Spectrum-6 at **102.4 Tb/s** per chip with 200G PAM4 SerDes (NVIDIA platform blog). NVIDIA says Spectrum-X Ethernet Photonics, its co-packaged optics switch line, is in production and delivers **5x** better power efficiency than networks using traditional transceivers (May 2026 release).

BlueField-4 DPUs offload networking, storage and security at up to **800Gb/s** (NVIDIA). For which fabric does what, see Inside Deep Tech's NVLink, InfiniBand, and UALink guide.

## Groq 3 LPX and the Rubin CPX question

The biggest change since the 2025 roadmap is the decode accelerator. NVIDIA's GTC 2026 release added the Groq 3 LPX rack: **256** LPU processors with **128GB** of on-chip SRAM and **640 TB/s** of scale-up bandwidth, deployed alongside NVL72 and slated for availability in the second half of 2026.

NVL72 handles prefill and decode attention while LPUs run the latency-sensitive FFN decode loop via NVIDIA Dynamo's Attention-FFN Disaggregation, per NVIDIA's scale-up blog. NVIDIA claims up to **35x** higher inference throughput per megawatt for trillion-parameter models with the combination (GTC 2026 release). See Inside Deep Tech's Groq LPU full guide for the LPU side.

Rubin CPX is the open question. NVIDIA announced it in September 2025 with **30 petaflops** of NVFP4, **128GB** of GDDR7 and end-of-2026 availability (NVIDIA). It was absent from the GTC 2026 keynote and slides (Tom's Hardware), and later analyst reports of a redesigned CPX haven't been confirmed by NVIDIA. Treat any CPX-based plan as speculative until NVIDIA publishes a new spec.

For the full CPX story, from the 2025 NVL144 CPX rack to Ian Buck's comments at GTC 2026 and the unconfirmed HBM4 revival report, read Inside Deep Tech's NVIDIA Rubin CPX full guide.

## Performance claims, and what they're measured on

NVIDIA's headline claims are big, and every one has a footnote. On the Vera Rubin NVL72 page, NVIDIA claims:

- One-tenth the cost per million tokens versus GB200 NVL72, measured on Kimi-K2-Thinking at 32K/8K input and output lengths (NVIDIA).
- Up to 10x more tokens per megawatt than GB200 NVL72 on the same model and settings.
- One-fourth the GPUs to train a 10T-parameter MoE model on 100T tokens in one month, labeled "projected performance subject to change" (NVIDIA).
- 30x higher throughput per megawatt and 35x lower token cost versus Grace Blackwell Ultra, stated on the Q2 fiscal 2027 earnings call.

Those claims use different baselines and NVIDIA-selected models. They're useful for direction, not for a purchase order.

The first independent check arrived on September 16, 2026. In MLPerf Inference v6.1, Vera Rubin NVL72 appeared in the preview category on DeepSeek-R1 and Qwen3-VL, and NVIDIA reported up to 3.7x higher Qwen3-VL throughput and up to 2.5x higher DeepSeek-R1 throughput than GB300 NVL72 (NVIDIA). Nebius submitted a 36-GPU preview system that posted 591,368 tokens per second on DeepSeek-R1 in the server scenario (Nebius). MLCommons' working group chairs note that the round's biggest gains came from that preview system (MLCommons). Preview is MLPerf's category for systems not yet generally available at submission, so read these as a first look, not a final ranking.

Run your own model, sequence lengths and batch sizes before you sign a capacity plan.

## Facility planning: power, cooling, and what NVIDIA hasn't published

NVIDIA's spec table doesn't list a per-rack power figure for Vera Rubin NVL72 in the sources reviewed here. What it does publish is a reference design: about 40K Rubin GPUs in a 100 MW AI factory using NVIDIA DSX with MaxLPS.

NVIDIA says DSX MaxLPS can provision up to 40% more GPUs in the same power budget, and the GTC release says DSX Max-Q enables 30% more AI infrastructure in a fixed-power data center. Both are provisioning claims, not lower chip power.

Longer term, Kyber racks are where power architecture breaks from today's halls. Inside Deep Tech's 800 VDC power distribution guide explains why NVIDIA's Kyber generation pushes facilities toward 800-volt DC distribution.

## Roadmap: Rubin Ultra NVL576, Kyber NVL144, and Feynman

NVIDIA's POD blog lays out the next two steps. Vera Rubin Ultra adds a two-layer all-to-all NVLink topology that links eight MGX NVL racks, each with **72** Rubin Ultra GPUs, into a **576**-GPU domain using copper and direct optical connections.

Kyber is the next MGX rack design. It doubles the NVLink domain per rack to **144** GPUs and will first ship with Vera Rubin Ultra as a standalone NVL144, giving Rubin Ultra three scale-up options: NVL72, NVL144 and NVL576. Eight Kyber racks then form an NVL1152 domain for the Feynman generation (NVIDIA).

So "NVL144" is coming back, this time as a Kyber rack with Rubin Ultra. NVIDIA gives no Rubin Ultra ship date in the sources reviewed here, so treat dates in secondary coverage as unconfirmed.

For the rack side by side with the Blackwell systems it replaces, with memory, NVLink, power and cooling compared line by line, read Inside Deep Tech's Vera Rubin NVL72 vs GB300 vs GB200 NVL72 comparison.

For the full story on the next step, including the GTC 2025 and GTC 2026 specs, 800 VDC power, liquid cooling and the reported Kyber schedule changes, read Inside Deep Tech's Rubin Ultra and Kyber full guide.

Feynman, due in 2028, replaces Vera with the new Rosa CPU and adds stacked GPU dies and NVLink 8 over co-packaged optics. Inside Deep Tech's NVIDIA Feynman full guide separates what NVIDIA has confirmed from what has only been reported.

The main rack-scale alternative to Vera Rubin NVL72 is AMD Helios, with 72 MI455X GPUs and 31TB of HBM4 per rack. For a side-by-side on compute, memory, fabric and customers, read Inside Deep Tech's AMD Helios full guide.

For a generation-by-generation table of Rubin, Rubin Ultra and Feynman, with status labels for every figure, see Inside Deep Tech's Rubin vs Rubin Ultra vs Feynman comparison.

## Who should buy Vera Rubin now, and who should wait

Buy or reserve Vera Rubin NVL72 capacity when several of these are true:

- You train or serve large MoE models where 288 GB of HBM4 per GPU removes KV-cache offload.
- Your hall already runs liquid cooling and can supply 45°C coolant at NVL72 density.
- You already run GB200 or GB300 racks and can reuse MGX logistics and CUDA tooling.

Wait, or stay on Blackwell, when any of these dominate:

- Your models fit comfortably in Blackwell memory and you're compute bound, not memory bound.
- You can't get allocation; NVIDIA expects supply to remain a bottleneck through fiscal 2028.
- Policy requires a second GPU source, which puts AMD Instinct on the shortlist.

## Honest limits

The real limits as of November 2026:

- Supply: NVIDIA expects supply to remain a bottleneck at least through the end of fiscal 2028.
- Memory cost: NVIDIA describes extreme pricing conditions in memory, and that flows into rack prices.
- Spec ambiguity: NVIDIA's own pages list 19.2 TB/s or 22 TB/s per GPU, and 216 or 260 TB/s of NVLink per rack.
- Power: no per-rack power figure on NVIDIA's public spec table.
- Rubin CPX: status unconfirmed after its absence from GTC 2026.
- Groq 3 LPX is new, with availability in the second half of 2026 and no production track record yet.
- Performance multipliers are vendor projections on vendor-chosen models and baselines.

Related deep dives: the packaging under each Rubin package is covered in the TSMC CoWoS guide and the hybrid bonding guide; the warm-water loop in the data center liquid cooling guide; Spectrum-X Photonics in the co-packaged optics and silicon photonics guides; and the lithography behind leading-edge GPU dies in the EUV lithography and High-NA EUV guides. For how Rubin sits against other inference silicon, see the best AI inference chips of 2026.

## Frequently asked questions

#### What is NVIDIA Vera Rubin?

NVIDIA Vera Rubin is NVIDIA's AI platform after Blackwell, built from seven chips including the Rubin GPU and Vera CPU. Its flagship system, Vera Rubin NVL72, links 72 Rubin GPUs and 36 Vera CPUs over NVLink 6.

#### When does Vera Rubin ship?

Production shipments began in August 2026, per NVIDIA's Q2 fiscal 2027 earnings call. The earnings release said racks were running at partners including CoreWeave, Google Cloud, Microsoft Azure, Oracle Cloud Infrastructure and Nebius. CoreWeave announced availability on September 30, 2026 (CoreWeave), and OCI announced general availability on October 27, 2026 (NVIDIA).

#### Is Vera Rubin NVL144 the same as Vera Rubin NVL72?

For the first-generation rack, effectively yes. NVIDIA's 2025 materials counted 144 Rubin GPUs alongside 36 Vera CPUs, while 2026 materials count 72 dual-die packages. The Kyber-based NVL144 coming with Rubin Ultra is a different, denser rack (NVIDIA).

#### How does Vera Rubin compare with GB200 NVL72?

NVIDIA claims one-tenth the cost per million tokens and up to 10x more tokens per megawatt versus GB200 NVL72 on Kimi-K2-Thinking, per its product page. Per-GPU HBM bandwidth rises 2.8x over Blackwell, per the architecture blog.

#### What happened to Rubin CPX?

Its status is unconfirmed. NVIDIA announced Rubin CPX in September 2025 for end-of-2026 availability, but it didn't appear in the GTC 2026 keynote, Tom's Hardware reported, and NVIDIA now emphasizes Groq 3 LPX for low-latency decode.

#### Does Vera Rubin need liquid cooling?

## What to do next

If you're planning 2027 capacity, the work this quarter is concrete. Get OEM rack power and coolant specs in writing, benchmark your own models against the conservative spec column, and lock HBM-heavy configurations early while supply remains constrained.

The next decision point is Rubin Ultra. The facility you build for NVL72 now decides whether Kyber is an upgrade or a rebuild.
