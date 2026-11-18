# Step 5 — Track C: Huawei + Chinese Hardware + Intel CPUs (Feb 1 → Sep 22, 2026)
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 5 (Serveurs & hardware), Track C
**Coverage window:** February 1, 2026 → September 22, 2026
**Topics:** Huawei Ascend (910C/910D/920), CloudMatrix 384, Chinese AI chips (Cambricon, Moore Threads, Biren, MetaX), Intel Xeon 6/Clearwater Forest, Gaudi 3/Jaguar Shores, Panther Lake, CPU market share, memory

### Provenance legend
- **[official]** — vendor's own blog, docs, filings, earnings statements.
- **[vendor-reported]** — figure claimed by the vendor without independent audit.
- **[independent]** — reputable third-party press or independent measurement (SemiAnalysis, Mercury Research, TrendForce, Bloomberg, Reuters, Tom's Hardware, ServeTheHome, EE Times, DIGITIMES, SiliconANGLE).
- **[secondary]** — lower-tier press, blogs, analyst summaries; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

---

## 1. HUAWEI ASCEND

### 1.1 Ascend 910C — current flagship in production

- **Architecture:** dual-chiplet accelerator — two Ascend 910B dies packaged together — fabricated on **SMIC's N+2 7nm-class process** (no EUV) **[independent — Tom's Hardware]**. DaVinci NPU architecture, stacked HBM2E memory (8 stacks per package per TechPowerUp; 128 GB total per the LinkedIn/TechInsights summary) **[independent/secondary]**.
- **Performance (vendor-reported via press):** up to **780 TFLOPS dense BF16** per chip; package power ~**350 W**; chip-to-chip interconnect bandwidth 400 GB/s **[independent — Tom's Hardware; secondary — TechPowerUp]**.
- **Real-world standing:** DeepSeek research put Ascend 910C at roughly **60% of NVIDIA H100 inference performance** — despite only ~25% of the FLOPs — because inference is memory-bandwidth-bound **[secondary — SDxCentral, citing DeepSeek research]**. Yields reported near **40%** on SMIC 7nm in early 2025, at which point the line was described as profitable — "a crisis at TSMC, a milestone in Shenzhen" **[secondary — LinkedIn analysis]**.
- **2026 shipments:** Bloomberg (Apr 2025, context) expected Huawei to ship **over 800,000 Ascend 910B and 910C chips in 2025**, including to state telecom carriers and ByteDance **[independent — TweakTown citing Bloomberg]**. For 2026, Bloomberg (Dec 2025) reports **Huawei is preparing to double output** of its most advanced AI chips **[independent — via i3investor/Bloomberg relay]**.
- **Export-control context:** NVIDIA's H20 was effectively banned from China in April 2025 (NVIDIA took a ~$5.5B charge); Jensen Huang said in November 2025 that NVIDIA is "effectively blocked" from China; the Trump administration was reportedly considering allowing H200 sales, with no guarantee Beijing would permit adoption **[independent — SDxCentral; secondary — Bloomberg relay]**.

### 1.2 Ascend 910D — H100-class successor

- **Bloomberg (April 2026 reporting, relayed by TweakTown):** Huawei's Ascend 910D targets performance **matching or exceeding NVIDIA's Hopper H100** (2022 generation); first batch of samples expected in **late May**, development "still at an early stage" **[independent — Bloomberg via TweakTown]**.
- An unreliable community source (Medium) claims 910D uses a 5nm-class process, 4-die packaging, FP8 support, with volume production in Q2–Q3 2026 — **treat as [unverified]**.
- No credible first-party 910D announcement or shipment confirmation found as of Sep 22, 2026 **[unverified]**.

### 1.3 Ascend 920 — next-generation (general-purpose direction)

- **Unveiled April 2025** at a Huawei partner conference, days after NVIDIA's H20 export-license news **[independent — SDxCentral, citing DigiTimes Asia]**.
- Reported specs (DigiTimes/TechPowerUp): **SMIC 6nm** process; training-focused **Ascend 920C** delivering **>900 TFLOPS BF16** per card (~30–40% over 910C); **HBM3, 4,000 GB/s** memory bandwidth (up from 910C's 3,200 GB/s); PCIe 5.0 support; refined tensor engines for Transformer/MoE; **30–40% training-efficiency improvement** projected over 910C **[secondary — TechPowerUp; techhounder]**.
- Mass production was expected in **H2 2025** per DigiTimes — **no verified 2026 production/shipment confirmation located**; treat 920 availability as **[unverified]**.
- Huawei is reportedly repositioning Ascend 920 as a **general-purpose GPU** (not AI-only), a strategic pivot toward competing with NVIDIA across broader compute workloads **[secondary — techhounder]**.
- ⚠️ Some outlets conflate "Ascend 920" with "Ascend 920C" naming; keep the distinction flagged.

### 1.4 CloudMatrix 384 — rack-scale answer to GB200 NVL72

- **What it is:** 384 Ascend 910C chips across **16 racks (12 compute + 4 networking)**, all-to-all topology, showcased at WAIC Shanghai and the April 2025 partner conference **[independent — SDxCentral]**.
- **SemiAnalysis analysis (Apr 2025):** ~**300 PFLOPS BF16** total (vs 180 for GB200 NVL72); **49.2 TB aggregate HBM** (vs 13.8 TB on NVL72, ~3.6×); **2.1× scale-up bandwidth** and **5.3× scale-out bandwidth** vs NVL72; **6,912 pluggable optical transceivers (LPO @ 800G)** forming an all-optical all-to-all fabric with >5.5 Pbps aggregate internal bandwidth **[independent — SemiAnalysis, via TweakTown/SDxCentral/winbuzzer]**.
- **The cost:** ~**559 kW** peak power draw — roughly **4×** the GB200 NVL72 — plus 5× the chip count and 16× the floor space. SemiAnalysis's one-line logic: "having five times as many Ascends more than offsets each one being a third the performance of a Blackwell" **[independent — SemiAnalysis]**.
- **Economics:** electricity in parts of China fell to ~**$56/MWh in early 2025** (from ~$91/MWh in 2022), making power-hungry systems viable **[secondary — winbuzzer]**.
- **Software:** Huawei's **CANN** programming environment and **MindSpore** framework, with a translation layer ingesting PyTorch/TensorFlow graphs; Huawei plans to open-source more of the toolchain **[independent — Tom's Hardware]**.
- SemiAnalysis also flagged supply-chain reality: while Ascend *can* be fabbed at SMIC, the chip remains "global" — HBM from Korea, wafer production links to TSMC, tens of billions in fab equipment from the US/Netherlands/Japan — describing it as "aggressive skirting of export controls" **[independent — SemiAnalysis]**.

### 1.5 Huawei 2026 announcements (other)

- No verified new Ascend consumer/datacenter product launch at Huawei's 2026 flagship events was located in this research pass beyond the 920/910D roadmap items above — flag as a **coverage gap**; the Ascend story in-window is dominated by 910C volume ramp and CloudMatrix deployments.
- **Kunpeng server CPUs:** no in-window Kunpeng 930/935 launch news verified in this pass — **gap**; re-verify against Huawei's official server pages.

---

## 2. CHINESE AI CHIPS — CAMBRICON, MOORE THREADS, BIREN, METAX

### 2.1 Cambricon — the production leader

- **2026 production plan (Bloomberg, Dec 2025):** Cambricon plans to deliver **~500,000 AI accelerators in 2026** — more than tripling the ~142,000 units Goldman Sachs estimates for 2025 — including up to **300,000 units of flagship Siyuan 590 and 690** **[independent — Bloomberg via TrendForce/TechRadar/i3investor]**.
- **Process:** SMIC **N+2 7nm**; yields on the 590/690 reportedly only **~20%** — four of five dies unusable — and SMIC advanced-node capacity is the binding constraint **[independent — Bloomberg via TrendForce]**.
- **Siyuan 590:** in volume production since **Q3 2024**; ~80% of NVIDIA A100 performance; gains over Siyuan 290 estimated at **15–20%** via more advanced HBM and added compute units (same N+2 node) **[independent — TrendForce; secondary — ainvest]**.
- **Siyuan 690:** still in testing; large-scale mass production may slip to **H2 2026** **[independent — TrendForce]**.
- **Commercial:** Cambricon held just ~1% of China's AI chip market in 2024 (behind NVIDIA, Huawei, AMD); revenue surged **14× in the September 2025 quarter**; market value up 9× since 2021; **RMB 4.6B revenue in the first three quarters of 2025** — multiples of other Chinese AI-chip startups **[independent — TrendForce; secondary — ainvest]**.
- **Customers:** ByteDance accounts for **>50% of current orders**; Alibaba in talks as a future customer **[independent — Bloomberg relay]**.
- ⚠️ **Cambricon denied the reports:** in a WeChat statement it said circulating media information on its products, customers and production forecasts is "all inaccurate," without specifying which outlets **[independent — communicationstoday citing Bloomberg]** — treat the 500K figure as **[secondary]** pending confirmation.

### 2.2 Moore Threads — first domestic-GPU IPO, now profitable (narrowly)

- **IPO:** listed on Shanghai STAR Market **December 5, 2025** as "China's first domestic GPU stock," raising **8 billion yuan (~$1.2B)**; shares rose **>500% in the first 8 trading sessions**, 5× on debut day **[secondary — CoinMarketCap; BigGo Finance]**.
- **2025 results:** revenue **RMB 1.45–1.52B** (+231–247% YoY); net loss narrowed to **RMB 950M–1.1B** from 1.6B a year earlier; SCMP, Jan 22, 2026 **[secondary — indexbox citing SCMP]**.
- **Q1 2026 (TrendForce, Apr 28, 2026, citing EE Times China):** revenue **RMB 738M (+155.35% YoY)**; first quarterly net profit **RMB 29.36M** attributable to shareholders (swinging from a loss) — though **ex-non-recurring profit was still a RMB 54M loss** (narrowed 60% YoY) **[independent — TrendForce]**.
- **Flagship MTT S5000** (4th-gen **Pinghu** architecture, mass production 2025): up to **1,000 TFLOPS dense AI compute** per card; full precision **FP8–FP64**; native hardware acceleration for Transformer and MoE architectures **[independent — TrendForce; secondary — TMTPost]**; designed to rival H100 for LLM training **[secondary — TMTPost; DIGITIMES]**.
- **March 2026:** **RMB 660M order** for the **KUAE intelligent-computing cluster** — a 10,000-GPU system "capable of end-to-end training of trillion-parameter models" **[independent — TrendForce]**.
- **MUSA Developer Conference (Dec 20–21, 2025):** unveiled **Huagang** next-gen architecture (FP4–FP64 full precision, +50% compute density, 10× energy efficiency, scaling to 100,000+ GPU clusters) underpinning **Huashan** (AI training+inference) and **Lushan** (graphics) products; **MUSA 5.0** full-stack platform; KUAE 2.0 cluster data at 10K-GPU scale: **60% training efficiency** (dense), **40%** (MoE), >90% effective training time, ~95% linear scaling efficiency; **DeepSeek R1 671B** run on S5000 with SiliconFlow: **>4,000 tokens/s prefill and >1,000 tokens/s decode per card** — all vendor-reported figures, treat as **[vendor-reported]** **[secondary — Jiemian Global]**.
- **Capital markets:** began Hong Kong listing process in **June 2026** ("A+H" dual platform); hired EY as auditor **[secondary — BigGo Finance]**.
- Founder/CEO **Zhang Jianzhong** (ex-NVIDIA China head); company founded 2020; 3-year R&D spend ~3.8B yuan vs cumulative revenue; top 2024 customer Baidu (42% of sales) **[secondary — TMTPost]**.

### 2.3 Biren — blockbuster Hong Kong IPO

- **Listed January 2, 2026** on HKEX (**HKG: 6082**); raised **HK$5.58B (~$717M)**; retail oversubscription **2,348×**; shares spiked **+119% intraday**, closed **+76%** at HK$19.60 offer price **[secondary — financialcontent/tokenring; TrendForce]**.
- **Products:** **BR100** positioned as the domestic alternative to NVIDIA's high-end chips; BR104 successor in pipeline (per IPO-era coverage) **[secondary]**.
- **Pre-IPO:** June 2025 round of **CNY 1.5B** led by state-backed Guangdong/Shanghai investors; pre-IPO valuation **CNY 14B**; backers include Qiming, IDG Capital, Hillhouse, Walden, Ping An **[independent — TrendForce]**.
- Founded 2019, fabless general-purpose GPU.

### 2.4 MetaX — STAR listing, then A+H

- **Listed December 17, 2025** on STAR Market (12 days after Moore Threads); raised **3.9B yuan (~$578.9M)**; IPO price 104.66 yuan/share; opening market cap **41.8B yuan (~$6B)**; P/S 56.4× vs peer average 127.4× **[secondary — MEXC; BigGo Finance]**.
- **Products:** **Xiyun C-series** chips; claims support for full-scale pretraining of **128B-parameter MoE models**; CUDA-compatible software stack **[secondary — TMTPost]**.
- **Financials (from IPO-era filings):** Q1 losses of 2.3B yuan; **8.9B yuan unsold inventory**; receivables at **192% of Q1 revenue** — flagged by analysts as turnover/payment risk **[secondary — TMTPost]**.
- **June 2026:** initiated Hong Kong listing ("A+H" dual capital platform) **[secondary — BigGo Finance]**.

### 2.5 Sector picture

- Listing wave: Moore Threads (STAR, Dec 5 2025), MetaX (STAR, Dec 17 2025), **Biren (HK, Jan 2 2026)**, **Iluvatar CoreX (HK, Jan 8 2026)**, **Enflame** STAR IPO accepted Jan 22, 2026 (first A-share IPO accepted for review in 2026) **[secondary — BigGo Finance]**.
- Top-five domestic GPU makers raised **>30B yuan in secondary offerings** (2026) **[secondary — BigGo Finance]**.
- Domestic substitution rate for AI chips: **2–3.6% (2022–2024) → projected 31% by 2029** **[secondary — BigGo Finance]**.
- **Common bottleneck:** all rely on SMIC N+2 as the most stable domestic advanced node; N+3/N+4 multi-patterning yields stuck ~20%; equipment/spare-part import restrictions limit capacity expansion **[independent — TrendForce]**.
- Memory (HBM/LPDDR) availability is a parallel constraint **[independent — Bloomberg relay via techplugged]**.

---

## 3. INTEL

### 3.1 Corporate / foundry 2026

- CEO **Lip-Bu Tan** (ex-Cadence chief) took the helm **March 2025**; aggressively restructured with Intel Foundry separation and 18A focus **[secondary — financialcontent]**.
- **US government stake:** $8.9B in CHIPS Act grants converted to a **9.9% equity stake (August 2025)** — the administration as stakeholder, not just grantor **[secondary — theeconomicrule citing Manufacturing Dive; financialcontent]**.
- **Intel 18A:** first leading-edge node with RibbonFET (GAA) + PowerVia (backside power); **Microsoft committed** as a foundry customer; **NVIDIA tested 18A via multi-project wafer runs** without committing volume (TrendForce) — one secondary source (financialcontent, Jan 2026) claims NVIDIA signed for 18A/14A, which is **conflicting/single-sourced — [unverified]**.
- **Stock:** ~**$128.76 on June 26, 2026** after a ~500% run under Tan (Fortune); analyst consensus Hold; bull targets ~$150 **[secondary — theeconomicrule]**.
- **Trump–Tan meeting Jan 8, 2026:** "national champion" framing; Advanced Manufacturing Investment Credit expected to rise **25% → 35%** for Ohio/Arizona mega-fabs **[secondary — financialcontent]** — treat policy specifics as **[secondary]**.

### 3.2 Xeon 6 family (Granite Rapids / Sierra Forest)

- **Granite Rapids (Xeon 6900P):** launched **Sep 24, 2024**; up to **128 Redwood Cove P-cores**; 504 MB L3; up to **500 W TDP**; 12 channels DDR5-6400; **MRDIMM up to 8800 MT/s** (market debut of the standard); 136 PCIe 5.0 lanes; CXL 2.0; AMX with MXFP4 support **[secondary — Wikipedia/techpowerup/dlcompare]**.
- **Sierra Forest (Xeon 6700E):** launched **Jun 4, 2024**; up to **288 Crestmont E-cores**; Intel 3; 8/12 DDR5 channels; 88 PCIe 5.0; CXL 2.0 **[secondary — Wikipedia]**.
- In-window 2026 news for the base Xeon 6 lineup is thin — the action moved to the successor (Clearwater Forest).

### 3.3 Clearwater Forest — Xeon 6+, launched June 1, 2026

- **Launched June 1, 2026 at Computex**; **first data-center processor on Intel 18A** **[secondary — Wikipedia]**.
- Up to **288 Darkmont E-cores** per socket; 2-socket configs; **12 channels DDR5-8000**; **96 PCIe 5.0 lanes**; **64 CXL 2.0 lanes**; up to 6 UPI links; 576 MB max L3; built-in accelerators (QAT, DSA, DLB, IAA); SGX/TDX **[secondary — Wikipedia]**.
- Succeeds Sierra Forest; shares I/O tiles with Granite Rapids; successor line points to **Diamond Rapids (2027)** — no 2026 Diamond Rapids/Panther Cove news located **[gap]**.

### 3.4 Gaudi 3 / Falcon Shores / Jaguar Shores — AI accelerator strategy

- **Gaudi 3** (launched fall 2024, Habana Labs): 64 TPCs + 8 MMEs; **128 GB HBM2e**; 24×200Gb Ethernet scale-out; SynapseAI stack. **Commercial miss:** Intel admitted (Nov 2024) it would miss its **$500M Gaudi 3 sales target** on software issues; beyond IBM, few major providers committed **[independent — TechCrunch]**.
- **Falcon Shores CANCELLED as a product (Jan 30, 2025):** interim co-CEO Michelle Johnston Holthaus announced Falcon Shores — the TSMC-3nm, ~1500W programmable GPU meant to succeed Gaudi 3 in late 2025 — would be an **internal test chip only**, not brought to market **[independent — CRN; TechCrunch; TweakTown]**.
- **Jaguar Shores:** the successor is now framed as a **rack-scale system-level solution**, not a standalone chip — Intel's lesson from Gaudi being "it's not enough to just deliver the silicon" **[independent — CRN]**.
- **2026 status:** no verified Jaguar Shores silicon/launch news in-window **[gap]**.
- **Crescent Island:** next-gen **inference-focused datacenter GPU (Xe3P)**, reported with 160 GB LPDDR5X, air-cooled, **sampling H2 2026** — sourced only from a Red Hat ecosystem GitHub repo; **treat as [unverified]**.

### 3.5 Panther Lake — Core Ultra Series 3 (client AI PC, Jan 2026)

- **Launched January 5, 2026 at CES**; **first consumer chip on Intel 18A**, designed and manufactured entirely in the US; RibbonFET + PowerVia **[independent — KitGuru; Micro Center]**.
- Up to **16 cores** (4 Cougar Cove P + 8 Darkmont E + 4 LP E-cores); **Xe3 "Celestial" iGPU** up to 12 Xe cores (Arc B390), matching a laptop RTX 4050 per Intel; XeSS 3 with multi-frame generation **[independent — KitGuru]**.
- **NPU 5: 50 TOPS** (Copilot+ compliant); **180 platform TOPS** total (CPU+GPU+NPU); 4.3× faster LLM inference than AMD XDNA2 (Intel claim — **[vendor-reported]**) **[independent — KitGuru; secondary — GitHub research note]**.
- Memory: **LPDDR5X-9600**, DDR5-7200; 25W base / 80W turbo TDP; Thunderbolt 4, Wi-Fi 7, PCIe 5.0 (12/20 lanes) **[secondary]**.
- Intel claims +50–60% multithreaded perf gen-on-gen at 25W; "battery life king" Core Ultra X9 388H with up to 27h Netflix streaming **[vendor-reported]**.
- **200+ system designs** planned (ASUS Zenbook, MSI Prestige/Titan/Stealth, Samsung Galaxy Book6, Lenovo Yoga Slim 7i) **[secondary]**.
- CES demo: **70B-parameter LLM running locally** with 32K context on a thin-and-light reference design via unified memory (up to 128 GB allocatable to AI tasks) **[vendor-reported — financialcontent]**.
- Intel positioning: "Agentic AI PCs" running autonomous agents on-device **[secondary]**.

---

## 4. CPU LANDSCAPE 2026 — EPYC vs XEON, MEMORY

### 4.1 Market share (Mercury Research)

- **Q1 2026:** AMD hit **32.6% of the x86 CPU market** (units, all segments) — all-time high, up from 27.1% a year earlier; Intel 67.4% (down from 72.9%) **[secondary — TweakTown citing Mercury Research]**. AMD **server unit share 33.2%** (up from 27.2%); **EPYC 46.2% of server CPU revenue** — record, up from 41.3% in Q4 2025 — while Intel held 53.8% of x86 server revenue and 54.9% of server units; **Arm servers at 17.7%** **[secondary — TweakTown; theoutpost; walaw/UBS]**.
- **Q2 2026:** AMD server unit share **34.5%** (from 27.3% a year earlier); in a head-to-head **EPYC vs Xeon SP** view, AMD approaches **46.4%** **[secondary — webpronews citing Mercury]**.
- ⚠️ Segment-definition conflicts exist between outlets (e.g., Q2 "overall x86 share 30.7%" vs Q1's 32.6%) — figures above are quoted as reported per outlet; treat cross-outlet comparisons cautiously **[secondary]**.
- **Economics:** server CPU shipments +10–20% YoY driven "almost entirely by AI data center demand"; Intel server shipments flat; **AMD Q1 data-center revenue $5.8B of $10.3B total (55.9%), +57% YoY** — surpassing Intel's DCAI segment ($5.1B) in the same quarter, a structural inflection **[secondary — TweakTown; walaw]**.
- Lisa Su's framing: moving toward a **1:1 CPU:GPU ratio** in AI infrastructure as agentic workloads grow **[secondary — TweakTown]**.
- **Arm:** server shipments nearly doubled; 17.7% share with growth expected after **Arm AGI** launch — details thin **[secondary]**.
- **EPYC generation:** no new EPYC generation launch (9006/Venice) verified in-window — **gap**; the installed story remains EPYC 9005 (Turin).

### 4.2 Memory: DDR5, MRDIMM, CXL, and the DRAM crunch

- **MRDIMM** debuted with Granite Rapids — up to **8800 MT/s** on Xeon 6900P, targeting memory-bandwidth-bound AI workloads **[secondary — dlcompare]**.
- **Clearwater Forest** pushes to **DDR5-8000** across 12 channels **[secondary — Wikipedia]**.
- **CXL 2.0** is standard on Xeon 6 and Clearwater Forest (64 lanes on the latter) for memory expansion/pooling; no verified CXL 3.0 deployment news in-window — **gap**.
- **The 2026 DRAM crunch:** chipmakers funneled production toward **HBM for AI accelerators**, leaving consumer-grade DRAM scarce and expensive; Mercury Research: "higher PC prices and limited GPU supplies are having a significant impact on end demand for desktop PCs" **[secondary — webpronews citing Mercury/The Register]**.
- Server DRAM pricing specifics for 2026 were not verified in this pass — **gap**; re-verify against DRAMeXchange/TrendForce contract-price series.

### 4.3 Other 2026 CPU/server-chip launches relevant to AI

- **Qualcomm Snapdragon X2 Plus** (client): up to 80 platform TOPS — cited as Panther Lake's competitor at CES 2026 **[secondary — Micro Center]**.
- **Apple M5-class silicon** referenced in local-inference contexts (Step 4 track B); no server CPU play.
- No verified **NVIDIA Grace CPU** refresh, **Amazon Graviton 5**, or **Google Axion** next-gen launch news was collected in this pass — **gaps**; worth a dedicated sweep if server-CPU completeness is required.

---

## 5. MASTER TIMELINE (Feb 1 → Sep 22, 2026)

- **Jan 2, 2026** — Biren lists on HKEX (6082); +119% intraday, closes +76%; raises HK$5.58B **[secondary]**.
- **Jan 5, 2026** — Intel launches Panther Lake / Core Ultra Series 3 at CES (Intel 18A) **[independent]**.
- **Jan 8, 2026** — Intel Iluvatar CoreX lists on HKEX **[secondary]**.
- **Jan 8, 2026** — Trump–Tan "national champion" meeting; Intel +10.5% **[secondary]**.
- **Jan 22, 2026** — Enflame STAR IPO accepted (first A-share IPO of 2026) **[secondary]**.
- **Feb–Mar 2026** — Cambricon Siyuan 690 in testing; H2 2026 mass-production target **[independent]**.
- **Mar 2026** — Moore Threads wins RMB 660M KUAE 10K-GPU cluster order **[independent]**.
- **Apr 28, 2026** — Moore Threads reports Q1 2026: first quarterly profit (RMB 29.36M attributable) **[independent]**.
- **May 2026 (expected)** — Huawei Ascend 910D first samples (Bloomberg reporting) **[independent — via relay]**.
- **Jun 1, 2026** — Intel Clearwater Forest (Xeon 6+) launches at Computex (Intel 18A, 288 E-cores) **[secondary]**.
- **Jun 2026** — Moore Threads and MetaX begin Hong Kong "A+H" listing processes **[secondary]**.
- **Jun 26, 2026** — Intel stock ~$128.76 (Fortune) **[secondary]**.
- **H2 2026 (planned)** — Intel Crescent Island inference GPU sampling **[unverified]**.
- **Q2–Q3 2026 (rumored)** — Ascend 910D mass production **[unverified]**.

---

## 6. OPEN VERIFICATION ITEMS

1. Ascend 920 production/shipment status in 2026 — no confirmation found **[unverified]**.
2. Ascend 910D specs (5nm? 4-die? FP8?) and mass-production timeline — Bloomberg only confirms H100-targeting samples **[secondary]**.
3. Huawei's "double output in 2026" — Bloomberg private-source reporting **[secondary]**.
4. Cambricon's 500K-unit 2026 plan — company publicly called reports "inaccurate" **[secondary]**.
5. SMIC N+2 yield figures (20% Cambricon 590/690; 40% Huawei 910C) — single-source relays **[secondary]**.
6. Moore Threads' 1,000 TFLOPS/S5000, KUAE 2.0 efficiency (60%/40%), DeepSeek R1 inference numbers — vendor-reported **[vendor-reported]**.
7. MetaX Q1 financials (2.3B yuan loss, 8.9B inventory) — IPO-filing-era reporting **[secondary]**.
8. Intel Crescent Island existence/specs — single GitHub-ecosystem source **[unverified]**.
9. NVIDIA as Intel 18A/14A volume customer — conflicting/secondary **[unverified]**.
10. Clearwater Forest specs — Wikipedia infobox (secondary); cross-check with Intel ARK **[secondary]**.
11. Panther Lake 70B-local-LLM demo, 27h battery, 4.3× vs XDNA2 — Intel claims **[vendor-reported]**.
12. Mercury Research share figures — consistent across relays but primary report not fetched **[independent-via-secondary]**.
13. Kunpeng 930/935, EPYC 9006/Venice, CXL 3.0 deployments, server DRAM pricing — **gaps, not located**.

## 7. COLLECTION METADATA

- **Research date:** September 22, 2026.
- **Priority sources used:** ServeTheHome-class coverage via TechPowerUp/TweakTown/Tom's Hardware; SemiAnalysis (CloudMatrix); Mercury Research (share); TrendForce/DIGITIMES relays; TrendForce news; Bloomberg relays; official-vendor claims via press.
- **Conventions kept:** every fact carries a provenance tag; uncertain items numbered in §6; no identifiers guessed; prices are dated snapshots.
