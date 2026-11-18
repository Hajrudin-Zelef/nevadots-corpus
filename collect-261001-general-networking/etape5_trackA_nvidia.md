# Step 5 — Track A: NVIDIA GPUs (B200/B300/Rubin), DGX Systems & Networking
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 5 (Servers & hardware), Track A
**Coverage window:** February 1, 2026 → September 22, 2026
**Status:** Research snapshot. Prices are dated; re-verify before use. Street prices and cloud rates move weekly.

### Provenance legend
- **[official]** — NVIDIA's own blog, docs, GTC keynote, datasheet, or regulatory filing.
- **[vendor-reported]** — figure claimed by NVIDIA (benchmarks, throughput, ROI) without independent audit.
- **[independent]** — reputable third-party press (Bloomberg, Reuters, CNBC, DCD, EE Times, SemiAnalysis, The Register, Tom's Hardware, ServeTheHome, VideoCardz) or independent measurement (LMSYS, Artificial Analysis).
- **[secondary]** — lower-tier press, blogs, analyst summaries, GitHub research memos; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

---

## 1. Blackwell: B200 / B300 GPU specifications

### 1.1 B200 (Blackwell)
- **Architecture:** Blackwell, TSMC 4NP process, dual-die package (~208B transistors) [secondary]
- **Streaming multiprocessors:** 148 SM (TechPowerUp lists B200 SXM 192 GB as 148 SM / 18,944 CUDA cores) [secondary]
- **Tensor cores:** 5th generation [secondary]
- **Compute:**
  - FP32: ~75 TFLOPS
  - FP16/BF16 tensor: ~2,250 TFLOPS (dense) [secondary — community spec tables]
  - FP8 tensor: ~4,500 TFLOPS dense (~9 PFLOPS sparse) [secondary]
  - FP4 (NVFP4): ~9 PFLOPS dense, ~18–20 PFLOPS sparse [secondary — figures vary across sources: 9/18 (sparse 2x) vs 9/20 per Spheron; treat as vendor-derived]
  - FP64: 37 TFLOPS [secondary]
- **Memory:** 192 GB HBM3e (one source table lists 180 GB "usable"); 8 TB/s bandwidth [secondary]
- **Interconnect:** NVLink 5, 1.8 TB/s per GPU; PCIe 5.0 [secondary]
- **TDP:** ~1,000 W class (configurable to 1,200 W per some tables) [secondary]
- **Launch status:** Announced GTC March 2024; volume shipping Q4 2024; widely deployed through 2025–2026 [secondary]
- **Pricing (2026 snapshot):**
  - Single GPU buy: ~$30,000–$50,000 [secondary — https://intuitionlabs.ai/articles/nvidia-ai-gpu-pricing-guide]
  - Cloud rental: $2.12–$6.04/hr (neo-cloud), on-demand median ~$5.50/hr (May 2026, Spheron); reserved 36-month pricing dropped to $2.25/GPU/hr (April 2026); spot from $2.12/hr [secondary — https://gpuaas.com/blog/h100-vs-h200-vs-b200-which-gpu-to-rent-2026]
  - **Residual value hit 158% in mid-2026 — 58% above launch price** [secondary — https://tech-insider.org/nvidia-b200-residual-value-158-percent-2026/]
  - B200 rental index surged 24% in March 2026 before pulling back (Silicon Data) [secondary]

### 1.2 B300 / Blackwell Ultra
- **Architecture:** Blackwell Ultra, TSMC 4NP, 208B-transistor dual-die [secondary]
- **Streaming multiprocessors:** 160 SM [secondary — per NVIDIA Technical Blog]
- **Compute:**
  - FP4 (NVFP4): ~15 PFLOPS dense (~28–30 PFLOPS sparse est.) [secondary]
  - FP8: ~4.5–13.5 PFLOPS depending on source (figures conflict — flagged)
  - FP16/BF16: ~2.2 PFLOPS [secondary]
  - ⚠️ **FP64 collapses to ~1.25 TFLOPS** (vs 37 TFLOPS on B200) — B300 trades nearly all double-precision performance for FP4 headroom [secondary — https://ifactoryapp.com/sap-integration/on-prem-ai/nvidia-dgx-b200-b300-blackwell-systems]
- **Memory:** 288 GB HBM3e; 8 TB/s bandwidth [secondary]
- **Interconnect:** NVLink 5, 1.8 TB/s per GPU; PCIe 6.0 [secondary]
- **TDP:** 1,200–1,400 W class (DGX B300 system draws 14.5 kW across 8 GPUs) [secondary]
- **Launch status:** Partners shipping from 2H 2025; DGX B300 shipping January 2026 [secondary]
- **Pricing (2026 snapshot):**
  - Single GPU: ~$53,000 [secondary]
  - DGX B300 (8-GPU system): $300,000–$350,000 [secondary]
  - Cloud rental: $3.27–$18.00/hr across 17 tracked providers [secondary — https://gpusmith.com/articles/en/h200-vs-b200-vs-b300-vs-mi325x]

### 1.3 DGX B200 vs DGX B300 systems
| Specification | DGX B200 | DGX B300 |
|---|---|---|
| GPUs | 8× B200 | 8× B300 |
| HBM3e per GPU | 192 GB | 288 GB (+50%) |
| Total system memory | 1.5 TB | 2.3 TB |
| FP4 dense per GPU | 9 PFLOPS | 15 PFLOPS (+67%) |
| FP64 per GPU | 37 TFLOPS | 1.25 TFLOPS |
| TDP per GPU | 1,000 W | 1,400 W |
| System peak power | ~10 kW | ~14 kW |
| NVLink | NVLink 5 | NVLink 5 |
| Cooling | Air or liquid (recommended) | Direct liquid cooling mandatory |
| System price (2026) | ~$280K–$320K | $300K–$350K |
| Lead time (Apr 2026) | 8–16 weeks | 12–20 weeks |
[secondary — https://ifactoryapp.com/sap-integration/on-prem-ai/nvidia-dgx-b200-b300-blackwell-systems]

### 1.4 GB200 / GB300 NVL72 rack-scale systems
- **GB300 NVL72:** 72 Blackwell Ultra GPUs + 36 Grace CPUs per rack; ~1.4 EFLOPS FP4 inference; ~140 kW rack power [secondary]
- Allocation at the top of the Blackwell stack remained constrained through 2026 [secondary]
- Rubin CPX marketing positions Vera Rubin NVL144 CPX at 7.5× the AI performance of GB300 NVL72 [vendor-reported]

---

## 2. Vera Rubin platform (2026)

### 2.1 Platform overview
- **Announcement timeline:** CES 2026 (January, Las Vegas) — formal platform announcement; **GTC 2026 (March 16, keynote)** — full production confirmed; partner availability H2 2026 [independent — DCD; secondary]
- **Silicon:** 336-billion-transistor platform on TSMC N3P (3nm) — nearly double Blackwell's density [secondary — https://blockeden.xyz/blog/2026/03/20/nvidia-gtc-2026-vera-rubin-gpu-architecture-depin-compute-bottleneck/; figure from secondary sources, not verified against NVIDIA]
- **Memory:** HBM4 (SK Hynix and Samsung); per-GPU memory bandwidth ~22 TB/s — nearly 3× Blackwell's 8 TB/s [secondary]
- **Performance claims:** 5× inference throughput vs Blackwell at rack level; up to 10× lower inference token cost; 4× fewer GPUs for MoE training vs Blackwell; MoE inference at ~1/7 the token cost of GB200 [vendor-reported]
- **⚠️ Supply:** HBM4 reported completely sold out through 2026 at GTC 2026; GPU lead times 36–52 weeks [secondary]

### 2.2 Rubin GPU (R100)
- 72 Rubin GPUs in NVL72; rack delivers 3.6 EFLOPS NVFP4 inference + 2.5 EFLOPS training → **~50 PFLOPS NVFP4 per GPU** (derived) [secondary]
- NVLink 6: 3.6 TB/s per GPU scale-up (2× Blackwell); 260 TB/s aggregate NVL72 fabric [secondary]
- Vera Rubin NVL72 power: 190–230 kW per rack (vs ~140 kW for GB300 NVL72); requires **800V DC power delivery** rather than the 48V standard [secondary]
- Full liquid cooling (dry cooling), no fans; NVIDIA claims 47 minutes from truck arrival to power-on [secondary — https://finance.biggo.com/news/202607220220_Nvidia_Vera_Rubin_NVL72_full_production]

### 2.3 Vera CPU
- 88 NVIDIA custom "Olympus" cores, 176 threads (Spatial Multi-Threading) [secondary — https://videocardz.com/newz/nvidia-vera-rubin-nvl72-detailed-72-gpus-36-cpus-260-tb-s-scale-up-bandwidth]
- 2 MB L2 per core; 162 MB unified L3 [secondary]
- Memory: up to 1.5 TB LPDDR5X at up to 1.2 TB/s (vs Grace: 480 GB at 512 GB/s) [secondary]
- NVLink-C2C: 1.8 TB/s (vs 900 GB/s on Grace) [secondary]
- PCIe Gen6 / CXL 3.1; confidential compute supported [secondary]

### 2.4 Vera Rubin NVL72 rack
- **Configuration:** 72 Rubin GPUs + 36 Vera CPUs, 18 compute trays, 9 NVLink switch trays, ~5,000 copper cables in NVLink spine (over 2 miles) [secondary]
- **Memory:** 20.7 TB HBM4 + 54 TB LPDDR5x; 1.6 PB/s HBM bandwidth [secondary]
- **Performance:** 3.6 EFLOPS NVFP4 inference; 2.5 EFLOPS training [vendor-reported]
- **Scale:** full Vera Rubin POD = 40 racks, 1,152 Rubin GPUs, ~60 EFLOPS NVFP4 [secondary]
- **Production status:** NVIDIA stated Rubin "in full production" in Q1 2026 (per GTC 2026 coverage) — earlier guidance had pointed to mass production in H2 2026; partner availability H2 2026 [independent/secondary — https://videocardz.com/newz/nvidia-vera-rubin-nvl72-detailed-72-gpus-36-cpus-260-tb-s-scale-up-bandwidth]
- **Cloud rollout (H2 2026 confirmed):** CoreWeave (first rack operational June 1, 2026), AWS, Google Cloud, Microsoft Azure, Oracle Cloud, Lambda, Nebius, Nscale [independent/secondary — https://ts2.tech/en/nebius-group-nbis-stock-jumps-after-nvidia-rubin-nvl72-plan-what-investors-watch-next/]
- **Reported customers:** OpenAI (deploying at scale Q3 2026 per Bloomberg reports), Meta, Dell; CoreWeave reported 10× token output vs Grace Blackwell generation [secondary — https://finance.biggo.com/news/202607220220_Nvidia_Vera_Rubin_NVL72_full_production]
- **Europe:** Bull (in partnership with Foxconn) announced European commercial availability, components manufactured in France and the Czech Republic [secondary — https://www.techtimes.com/articles/318651/20260618/nvidia-vera-rubin-nvl72-cloud-rollout-expands-europe-h2-deployments-near.htm]
- ⚠️ One secondary source claims AWS and Google Cloud will brand the Rubin R100 GPU as **"H300"** for catalog continuity — single-source, treat as [unverified]

### 2.5 Rubin CPX (prefill/context GPU)
- **Positioning:** new GPU category purpose-built for the prefill (context) phase of disaggregated LLM inference — million-token software coding, generative video, agentic workloads [independent — https://www.eetimes.com/nvidia-specializes-gpu-for-first-stage-of-transformer-inference/]
- **Silicon:** monolithic single die (departure from NVIDIA's dual-GPU packages) [independent — DCD, EE Times]
- **Compute:** up to 30 PFLOPS NVFP4 [official via press]
- **Memory:** 128 GB **GDDR7** (not HBM) — deliberate cost/throughput trade for context processing [official via press]
- **Media:** 4× NVENC + 4× NVDEC for video workflows [official via press]
- **Claims:** up to 3× faster attention performance vs GB300 NVL72 for long-context processing [vendor-reported]
- **Availability:** end of 2026 / late 2026 [official via press — https://www.datacenterdynamics.com/en/news/nvidia-launches-rubin-cpx-gpu-for-large-scale-inferencing/]
- **Vera Rubin NVL144 CPX rack:** 144 Rubin CPX GPUs + 144 Rubin GPUs + 36 Vera CPUs; 8 EFLOPS NVFP4 AI compute; 100 TB fast memory; 1.7 PB/s memory bandwidth; liquid-cooled MGX system; 7.5× AI performance vs GB300 NVL72 [official via press]
- **Economics claim:** NVIDIA's Ian Buck said $100M capex in CPX racks with Dynamo orchestration could deliver up to $5B in token revenue (30–50× ROI) [vendor-reported — https://www.eetimes.com/nvidia-specializes-gpu-for-first-stage-of-transformer-inference/]
- **Early adopters:** Cursor, Runway, Magic [secondary — https://www.datacenterdynamics.com/en/news/nvidia-launches-rubin-cpx-gpu-for-large-scale-inferencing/]

### 2.6 Rubin Ultra (2027) and Feynman (2028) roadmap
- **Rubin Ultra — 2027:** 600 kW rack-class system previewed at GTC 2026 [secondary — https://letsdatascience.com/blog/jensen-huang-walked-out-with-a-chip-doing-50-petaflops-the-ai-industry-held-its-breath]
  - NVL576 configuration: 576 Rubin-class GPUs [secondary — https://www.tweaktown.com/news/107645/nvidia-rubin-cpx-gpu-to-feature-128gb-gddr7-memory-launches-end-of-2026/index.html]
  - Up to 4 GPUs per module; HBM4E memory; NVIDIA Kyber networking (copper + co-packaged optics) [secondary]
  - ⚠️ One medium source describes "Vera Rubin Ultra" as 144 GPUs in vertically oriented compute trays — **conflicts with NVL576/576-GPU figure; flagged [unverified]**
- **Feynman — 2028 (first public preview at GTC 2026):**
  - Rosa CPU (named after Rosalind Franklin) [secondary]
  - LP40 LPU — next-generation language processing unit, continuing Groq LPU integration [secondary]
  - BlueField-5 DPU, ConnectX-10 (CX10) networking [secondary]
  - NVLink 8, Spectrum-7, co-packaged optics via NVIDIA Kyber [secondary]
  - TSMC A16 1.6nm process [secondary — https://letsdatascience.com/blog/jensen-huang-walked-out-with-a-chip-doing-50-petaflops-the-ai-industry-held-its-breath]

---

## 3. H100 / H200: availability and 2026 price trends

### 3.1 Purchase prices (2026)
- **H100:** $25,000–$40,000 per GPU [secondary — https://intuitionlabs.ai/articles/nvidia-ai-gpu-pricing-guide]
- **H200:** ~$31,000 per GPU; 8-GPU systems at ~$315,000 [secondary]
- A100 40 GB: ~$10–12K; A100 80 GB: ~$15–17K (for context) [secondary]

### 3.2 Cloud rental (2026)
- **H100:** $1.38–$12.29/hr range across providers; on-demand median $2.01–$3.33/hr; spot as low as $1.25/hr; average ~$3.11/hr early 2026 [secondary]
  - SemiAnalysis 1-year contract index (100+ market participants): $1.70/hr (Oct 2025) → **$2.35/hr (Mar 2026)** [independent]
  - Historical: $8–10/hr (Q4 2024 peak) → $5.50–7 (Q1 2025) → $3.50–4.50 (Q2 2025) → $2.85–3.50 (Q3–Q4 2025): 64–75% decline from peak [secondary]
- **H200:** 25–30% premium over H100; on-demand from $3.72/hr; dedicated bare-metal $2.14–$3.59/hr; range $3.50–$10.60/hr [secondary]
  - ⚠️ **H200 contract pricing rose ~40% between Oct 2025 and Mar 2026** even as B200 supply ramped — roughly half of tracked specialist providers reported no Hopper-class capacity coming off contract; demand outstripping supply across both Hopper and early Blackwell simultaneously [secondary — https://tech-insider.org/nvidia-b200-residual-value-158-percent-2026/]
- HBM memory scarcity (not GPU die availability) is now the primary price driver; H100/H200 contract pricing up ~40% in a six-month window while secondary-market older-gen pricing collapsed ~85% [secondary — https://intuitionlabs.ai/pdfs/data-center-gpu-pricing-2026.pdf]

### 3.3 Availability
- H100/A100/H200 cluster at 63–70% confirmed-stock rates across providers (rest provisioning-dependent) [secondary — AIMultiple]
- SemiAnalysis: "hunting for even 8 nodes (64 GPUs) of H100s or H200s is not easy; half the providers we asked were completely sold out" (early 2026); all capacity coming online until Aug–Sep 2026 already booked [independent — SemiAnalysis newsletter]
- H100 remains the cost-performance sweet spot and the default for training under 70B params in 2026; H200 is a drop-in rack replacement (same 700W TDP) tripling KV-cache headroom (141 GB HBM3e at 4.8 TB/s) [secondary — https://www.emma.ms/blog/nvidia-h200-vs-h100-comparison]

---

## 4. DGX systems: Spark, Station, Cloud

### 4.1 DGX Spark (GB10)
- **Specs:** NVIDIA GB10 Grace Blackwell Superchip (20-core Arm: 10× Cortex-X925 + 10× Cortex-A725); Blackwell GPU with 6,144 CUDA cores, 5th-gen Tensor, 4th-gen RT; up to **1 PFLOP (sparse FP4)** AI performance [secondary — https://peterfalkingham.com/2026/09/18/academic-tech-gigabyte-ai-top-nvidia-dgx-spark-review/]
- **Memory:** 128 GB LPDDR5x unified, 273 GB/s; model capacity ~200B params FP4 inference (405B with two units over ConnectX-7) [secondary]
- **Storage:** 4 TB NVMe Gen5, self-encrypting; **Networking:** 10 GbE RJ-45, ConnectX-7 @ 200 Gbps, WiFi 7; **Power:** 240W PSU (GB10 TDP 140W; 40–60W observed at wall under load) [secondary]
- **OS:** NVIDIA DGX OS (Ubuntu 24.04 base) [secondary]
- **Pricing:**
  - Launch MSRP: $3,999 → raised to **$4,699** (Feb 23, 2026) [secondary]
  - Sep 2026 street: NVIDIA marketplace $4,699; Amazon $4,679; retailer markups $4,999.99 (Micro Center via Amazon) to $6,155.99 (Best Buy/Newegg); UK £4,900–£5,485; MSI EdgeXpert variant spiked to $7,342 [secondary — https://www.eteknix.com/rtx-spark-prices-are-already-surging-weeks-before-launch/]
  - Price history: lowest $3,979.19 (Jan 30, 2026), highest $7,346.22 (Sep 15, 2026), average $4,757.26 [secondary — pricehistory.app]
- **Performance (independent):** LMSYS Org — GPT-OSS 20B in Ollama: 2,053 tok/s prefill, 49.7 tok/s decode (~1/4 of RTX Pro 6000 Blackwell WE; slower than single RTX 5090); Llama 3.1 8B @ batch 32: 368 tok/s decode; Sebastian Raschka: single-sample PyTorch inference "roughly on par with the 6× more expensive H100" [independent — https://gpusmith.com/articles/en/pdfs/nvidia-dgx-spark-review-specs-performance.pdf]
- **Issues:** thermal and power-delivery problems on early units — John Carmack publicly reported units capping near 100W of rated 240W and rebooting under sustained load; NVIDIA dev forum moderators confirmed "a known, documented problem across the platform" [independent/secondary]
- **Reception:** Tom's Hardware — "a well-rounded toolkit for local AI... but a pricey platform if you don't intend to use its features to the fullest" [independent]
- **Alternatives (Sep 2026):** Strix Halo 128GB ($2,600–3,600), OEM GB10 boxes (often below FE price), Radeon PRO W7900 ($3,700–3,999), Mac Studio M5 Max (from $2,499), RTX 5090 ($3,700–5,000 street) [secondary — https://memeburn.com/dgx-spark-alternatives/]
- **RTX Spark:** per eTeknix (Sep 21, 2026), "RTX Spark Mini PCs make their market debut" weeks out — prices already surging [secondary — treat launch timing as [unverified]]

### 4.2 DGX Station / DGX Cloud
- No verified 2026 DGX Station (desktop/workstation) refresh details were located in the fetched sources — **gap flagged** [unverified]
- DGX Cloud: NVIDIA's AI supercomputing cloud offering; no standalone 2026 pricing/availability news located in this research pass — **gap flagged** [unverified]
- Context: DGX Cloud was the vehicle for NVIDIA's "AI factory" enterprise push alongside Nemo/Dynamo software at GTC 2026 [secondary]

---

## 5. Networking: NICs, DPUs, switches, NVLink

### 5.1 ConnectX NICs
- **ConnectX-7:** 200 Gb/s; ships in DGX Spark as SmartNIC; still the volume NIC for H100/H200/B200-era deployments [secondary]
- **ConnectX-8 SuperNIC:** up to 800 Gb/s total (2× 400 Gb/s ports); PCIe Gen6 (up to 48 lanes), backward-compatible with Gen5/4/3; advanced QoS and congestion control; performance isolation for multi-tenant AI factories [official — Spectrum-X datasheet]
- **ConnectX-9 SuperNIC:** firmware v82.48.1000 GA **February 2026** [official — docs.nvidia.com]; up to 800 Gb/s per port over InfiniBand and Ethernet; up to **1.6 Tb/s throughput to Rubin GPUs**; programmable IO and intelligent congestion control; pairs with Spectrum-X Ethernet and Quantum-X800 [official]
- **ConnectX-10 (CX10):** roadmap — paired with Feynman (2028) [secondary]

### 5.2 BlueField DPUs
- **BlueField-3:** current-gen networking/security/storage offload; BlueField-3 SuperNIC delivers up to 400GbE RoCE with GPUDirect RDMA; HHHL form factor, sub-75W envelope (B3140H) [official/secondary]
- **BlueField-4:** announced at GTC 2026 as part of the Vera Rubin full-stack platform; extends infrastructure offload into agentic-AI storage via STX/CMX context-memory systems [secondary — https://github.com/hczhu/stock-research/blob/HEAD/memos/2026-07-13-nvidia-hardware-lineup-ai-factory-ecosystem.md]
- **BlueField-5:** future roadmap step paired with Feynman/Rosa-era infrastructure [secondary]
- Deployment note: in Spectrum-X reference topologies, BF3 supports single-plane operation only; north-south (management/storage) traffic handled by BlueField DPUs [secondary]

### 5.3 Spectrum switches and Spectrum-X
- **Spectrum-4 (SN5600/SN5600D/SN5610):** 64 ports of 800GbE in 2U; **51.2 Tb/s total throughput**; smart-leaf/spine/superspine and rail-optimized designs; 10–800GbE connectivity [official — Spectrum-X datasheet]
- **Spectrum-X800 platform:** SN5600 800Gb/s switch + BlueField-3 SuperNIC; optimized for multi-tenant genAI clouds with performance isolation per tenant [official — investor.nvidia.com]
- **Spectrum-6:** **102.4 Tb/s** switch for the Vera Rubin generation (2× Spectrum-4) [secondary]
- **Spectrum-X with co-packaged optics (CPO):** announced at GTC 2026 as "the world's first mass-produced CPO switch" — 5× optical power efficiency vs pluggable optics, 2 Tb/s per-port bandwidth, 10× network reliability [secondary — https://medium.com/@strategycheatsheet/key-takeaways-from-jensen-huangs-nvidia-gtc-2026-keynote-speech-aefbae9de255]
- **Spectrum-7:** roadmap with Feynman (2028) [secondary]
- Vera Rubin reference: Grace Blackwell = Spectrum-4 + ConnectX-8 (800 Gb/s); Vera Rubin = Spectrum-6 + ConnectX-9 (1.6 Tb/s) [secondary]

### 5.4 InfiniBand: Quantum-X800
- Quantum-X800 platform: Quantum Q3400 switch + ConnectX-8 SuperNIC; end-to-end 800 Gb/s; 5× bandwidth capacity and 9× SHARPv4 In-Network Computing (14.4 TFLOPS) vs previous generation; SHARPv4 supports FP8 [official — investor.nvidia.com]
- Rubin NVL144 CPX networking: Quantum-X800 InfiniBand or Spectrum-X Ethernet with ConnectX-9 SuperNICs [secondary]

### 5.5 NVLink scale-up fabric
| Architecture | NVLink gen | Per-GPU bandwidth | NVL72 aggregate |
|---|---|---|---|
| Hopper | NVLink 4 | 900 GB/s | n/a |
| Blackwell | NVLink 5 | 1.8 TB/s | 130 TB/s |
| Rubin | NVLink 6 | 3.6 TB/s | 260 TB/s |
| Feynman | NVLink 8 | (not disclosed) | (not disclosed) |
[secondary]
- NVIDIA adding optical scale-up alongside copper so NVLink domains can expand beyond a single rack (Kyber networking architecture) [secondary]
- NVLink Switch trays: 9 per Vera Rubin NVL72 rack [secondary]

### 5.6 200/400/800 GbE adoption
- 800GbE is the current datacenter standard for AI fabrics (Spectrum-4 64× 800GbE; ConnectX-8 800Gb/s) [official]
- ConnectX-9 pushes per-port 800 Gb/s and 1.6 Tb/s aggregate to Rubin GPUs [official]
- CPO switches (2 Tb/s per port) mark the transition toward 1.6T/3.2T-class optics with the Feynman generation [secondary]

---

## 6. GTC 2026 (March 16–19, San Jose) — key announcements

- **Vera Rubin full-stack platform:** full production confirmed; 7 chips, 5 rack-scale systems; partner availability H2 2026 [independent/secondary]
- **Feynman architecture:** first public preview — Rosa CPU, LP40 LPU, BlueField-5, CX10, Kyber co-packaged optics, NVLink 8, Spectrum-7 [secondary]
- **Groq acquisition:** NVIDIA confirmed acquisition of Groq's team and technology (LPUs integrated into Rubin/Feynman inference strategy) [secondary — https://acaderesearch.com/nvidia-gtc-2026-the-inference-inflection-vera-rubin-and-the-ai-factory-era/]
- **NemoClaw:** open-source enterprise AI agent framework (Wired leak March 10; briefings to Salesforce, Cisco, Google, Adobe, CrowdStrike) [secondary]
- **Dynamo:** inference operating system / cluster orchestration for disaggregated inference [secondary]
- **Nemotron 3:** Super/Ultra/Nano model families; Nemotron Coalition partners (Mistral, Perplexity, Cursor, Cohere) [secondary]
- **DLSS 5:** neural rendering beyond frame generation; "1,000,000× leap" in path-tracing performance claimed [vendor-reported/secondary]
- **Physical AI:** DRIVE Hyperion partnerships (Hyundai, BYD, Nissan) for Level 4 autonomy; T-Mobile AI-RAN partnership; Nokia deploying RTX PRO 4500 Blackwell Server Edition in AI-RAN base stations [secondary]
- **Spectrum-X CPO switch:** world's first mass-produced co-packaged-optics switch [secondary]
- **AWS partnership:** 1M+ NVIDIA GPUs across AWS regions (Blackwell + Rubin, RTX PRO Blackwell Server Edition, Groq 3 LPUs) [secondary]
- **Space-1 Vera Rubin:** orbital AI data centers concept [secondary — marketing announcement, treat as ambition]
- **Demand projection:** AI infrastructure demand doubled to **$1T through 2027** [vendor-reported]
- Context: keynote followed a ~14% stock pullback after a record $68.1B revenue quarter [secondary]

---

## 7. Verification log (open items)

1. **B200 usable memory** — 192 GB vs 180 GB "usable" conflict across secondary spec tables. [unverified]
2. **B300 FP8/FP4 sparse figures** — 9/18 vs 9/20 PFLOPS (B200) and ~28–30 PFLOPS sparse (B300) vary by source. [unverified]
3. **B300 FP8 compute** — 4.5 vs ~11–13.5 PFLOPS conflict. [unverified]
4. **336B-transistor Rubin figure** — from secondary sources; not verified against NVIDIA. [unverified]
5. **"H300" rebranding of Rubin R100** at AWS/Google Cloud — single secondary source. [unverified]
6. **Rubin Ultra configuration** — NVL576/576 GPUs vs 144 vertical-tray GPUs conflict. [unverified]
7. **OpenAI Q3 2026 Rubin deployment at scale** — Bloomberg via secondary relay. [unverified]
8. **DGX Station 2026 refresh** — no verified details located. [gap]
9. **DGX Cloud 2026 pricing/availability news** — no verified details located. [gap]
10. **RTX Spark launch timing** — "weeks before launch" per eTeknix Sep 21, 2026. [unverified]
11. **HBM4 sold out through 2026** — GTC 2026 secondary reporting. [unverified]
12. **CoreWeave 10× token output claim** — secondary relay of vendor/partner claim. [unverified]
13. **Space-1 orbital datacenters** — marketing concept, no deployment details. [unverified]
14. **Groq acquisition terms** — confirmed as announced but terms/price not verified in fetched sources. [unverified]

---

## 8. Collection metadata

- **Research date:** September 22, 2026
- **Sources prioritized:** NVIDIA official (docs.nvidia.com, investor.nvidia.com, datasheets), DCD, EE Times, VideoCardz, ServeTheHome-adjacent coverage, SemiAnalysis, Tom's Hardware, LMSYS, IntuitionLabs, GPUaaS, GPUSmith
- **Conventions:** all prices in USD unless noted; cloud rates are per-GPU-hour; "street" prices are retailer snapshots dated Sep 2026; sparse vs dense FLOPS always distinguished where sources allow
