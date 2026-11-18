# Step 5 — Server Vendors + AI Server Market + Data-Center Networking
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 5 (Servers & hardware), Track D
**Coverage window:** January 1 → September 22, 2026
**Topics:** AI server vendors (Supermicro, Dell, HPE, Giga Computing/Gigabyte, ASUS, MSI, xFusion), data-center networking (200/400/800 GbE, UEC/Super Ethernet, optical interconnect), AI server market figures
**Status:** Research snapshot. Prices, market shares, and revenue figures are dated snapshots as of September 22, 2026; re-verify before use. Vendor list prices are not publicly published (quote-based) for AI servers.
**Working drafts:** `_draft_etape5_supermicro_dell.md`, `_draft_etape5_hpe_others.md`, `_draft_etape5_networking.md`, `_draft_etape5_market.md` (kept alongside for audit).

### Provenance legend
- **[official]** — vendor's own press release, docs, earnings release, or regulatory filing.
- **[vendor-reported]** — figure claimed by the vendor without independent audit.
- **[independent]** — reputable third-party press (Bloomberg, Reuters, CNBC, WSJ, FT, SiliconANGLE, The Register, ServeTheHome, Data Center Dynamics) or independent measurement.
- **[secondary]** — lower-tier press, blogs, analyst summaries, community trackers; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

### Contents
- §1 Supermicro + Dell
- §2 HPE + Giga Computing + ASUS + MSI + xFusion
- §3 Data-center networking
- §4 AI server market figures
- §5 Open verification items (consolidated)

---

## §1 Supermicro + Dell

# Server Vendors — Wave 1: Supermicro + Dell (Research Report, as of 2026-09-22)

> Scope: 2026 product platforms/launches, specs, pricing, customer wins, AI revenue
> figures, governance/accounting updates, controversies. Provenance tag on EVERY claim:
> **[official]** = vendor press release / official IR or corporate page;
> **[vendor-reported]** = figure stated by the vendor in earnings/report;
> **[independent]** = press or tech media with its own reporting (ServeTheHome-style,
> Reuters, Bloomberg, DCD, Tom's Hardware, DIGITIMES…);
> **[secondary]** = aggregators, analyst blogs, market commentary (lower reliability);
> **[unverified]** = cannot be confirmed from the sources collected.
> Dates given only where a source states them; search-crawl dates are NOT treated as
> publication dates. Any figure without an explicit source state is flagged **[unverified]**.

---

## PART 1 — SUPERMICRO (Super Micro Computer, Inc., NASDAQ: SMCI)

### 1.1 2026 AI server platforms

Supermicro's 2026 AI portfolio is built around its Data Center Building Block Solutions
(DCBBS) approach and NVIDIA's Blackwell / Blackwell Ultra platforms **[official]**:

- **NVIDIA HGX B300 8-GPU systems** in two form factors: a compact **2OU (OCP)**
  variant and a **4U** variant, both with Supermicro **DLC-2** direct liquid cooling
  **[official]**. The 2OU OCP-based rack-scale design supports **up to 144 GPUs in a
  single rack** **[official]**. Each HGX B300 system carries **2.1 TB of HBM3e GPU
  memory**; cluster-level compute-fabric throughput is **up to 800 Gb/s** via integrated
  NVIDIA ConnectX-8 SuperNICs with NVIDIA Quantum-X800 InfiniBand or Spectrum-4
  Ethernet **[official]**.
- **NVIDIA HGX B300 NVL16** systems (announced March 2026 as part of the Blackwell Ultra
  wave), designed "for every data center" **[official]**.
- **NVIDIA GB300 NVL72** (Blackwell Ultra, rack-scale Grace-Blackwell) — launched in
  Supermicro's March 2026 Blackwell Ultra portfolio **[official]**.
- **NVIDIA GB200 NVL72** liquid-cooled systems (Blackwell) — launched late 2024, with
  samples shipping to customers in Q4 2024 and full production from end of Q4 2024
  **[official via secondary (Zhitong Finance, Oct 2024)]**; still part of the 2026
  lineup alongside GB300 NVL72 **[official]**.
- **NVIDIA HGX B200** 8-GPU systems (Blackwell, pre-Ultra generation) **[official]**.
- **GB200 NVL4 HPC** rack-scale solutions **[official]**.
- **Super AI Station** — deskside AI supercomputer based on NVIDIA GB300
  **[official]**.
- **NVIDIA RTX PRO 6000 Blackwell Server Edition** systems **[official]**.
- Planned **NVIDIA Vera Rubin NVL144 and Vera Rubin NVL144 CPX** platforms for 2026
  delivery, announced at NVIDIA GTC Washington, D.C. **[official]** (see 1.2).
- **4-GPU platforms**: Supermicro's March 2026 Blackwell Ultra announcement also covers
  smaller-node systems; the HGX B300 NVL16 (8-GPU NVL16-domain) is the mid-scale offer
  **[official]**. Standalone 4-GPU 2026 platforms are marketed via the
  accelerator/GPU-server portfolio **[official]**; no dedicated new 4-GPU Blackwell
  Ultra launch SKU was identified in collected sources — **[unverified]** whether a
  2026 4-GPU flagship exists beyond the RTX PRO 6000 workstation/server line.
- **SuperBlade**: in January 2026 Supermicro launched **SBI-622BA-1NE12-LCC**, a
  high-density liquid-cooled blade server "powered by dual Intel Xeon 6900 series
  processors," described by CEO Charles Liang as "the most core-dense SuperBlade we've
  ever created," with up to **3 TB of memory** support and 93% cabling reduction
  **[secondary (Barchart via greencitylivestock.com, Jan 2 2026)]**; target workloads:
  finance, climate modeling, scientific research **[secondary]**.

### 1.2 New launches with dates (2026)

| Date | Launch | Key specs | Source |
|---|---|---|---|
| Jan 2026 (reported Jan 2) | SuperBlade SBI-622BA-1NE12-LCC | Dual Intel Xeon 6900-series, liquid-cooled blade, up to 3 TB memory, 93% cabling reduction **[secondary]** | Barchart via greencitylivestock.com |
| Mar 2026 | Blackwell Ultra portfolio: HGX B300 NVL16 + GB300 NVL72, air- and liquid-cooled | 288 GB HBM3e per GPU; up to 800 Gb/s direct-to-GPU networking; 40°C warm-water (8-node rack) or 35°C warm-water (16-node double-density rack) operation with new CDUs; **up to 40% power reduction** and water conservation claimed **[official]** | Supermicro press release via europeansuntimes.com / worldnewsnetwork.net (article slug dated 2025-03-19 — note: slug suggests the underlying announcement may date to NVIDIA GTC March 2025 rather than 2026; TechPowerUp index lists it among Supermicro news) **[unverified date]** |
| Mar 2026 (MWC, Barcelona) | Pre-fabricated modular AI data centers with **SK Telecom + Schneider Electric**: pre-fab modules combining AI compute servers + power + cooling in one ready-to-deploy unit; SKT = operations, Supermicro = GPU servers, Schneider = power/infrastructure **[secondary]** | Ready-to-deploy building-block DC modules **[secondary]** | Barchart via business.dailytimesleader.com / markets.financialcontent.com |
| ~Oct–Nov 2025 (still 2026-relevant lineup) | HGX B300 4U + 2OU DLC-2 systems | DLC-2 captures **up to 98%** of system heat via liquid cooling; 45°C warm-water operation, no chilled water/compressors; up to 40% power savings; delivered as fully validated L11/L12 rack solutions **[official]** | stocktitan.net SMCI news page |
| ~Oct 2025 (NVIDIA GTC Washington, D.C.) | Expanded NVIDIA collaboration: 2OU HGX B300 (144 GPUs/rack), **Vera Rubin NVL144 + NVL144 CPX plans for 2026**, U.S.-manufactured **TAA-compliant** systems from San Jose, Super AI Station (GB300), GB200 NVL4 HPC; full-stack NVIDIA AI Factory for Government reference design **[official]** | Buy American Act-capable, TAA-compliant **[official]** | TechPowerUp / barchart.com / stocktitan.net (press-release text) |
| Aug 25–26, 2026 | **Cisco strategic collaboration**: Supermicro liquid- and air-cooled AI systems embedded in the **Cisco Secure AI Factory with NVIDIA**, validated and sold through Cisco and authorized channel partners; availability scheduled **from October 2026**; no disclosed financial transaction or equity stake **[official via vendor blog + independent (Axios)]** | Enterprise + AI-cloud channel access **[official]** | learn-more.supermicro.com; equityswarm.com citing Axios Aug 25, 2026 |

- **DLC-2 liquid cooling (2026 spec claims, [official])**: directly cools GPUs, CPUs,
  and memory with warm water up to 45°C inlet; up to 40% power-consumption reduction vs
  air cooling; up to 20% TCO reduction claimed in the DataVolt partnership context
  **[secondary]**; **250 kW per-rack heat-dissipation** capability; packs 8 NVIDIA
  Blackwell GPUs + 2 Intel Xeon CPUs into a single 4U server **[secondary (ainvest.com,
  May 2025)]**; captures up to 98% of heat through liquid **[official]**.

### 1.3 Pricing

- **No published per-server list prices** for Supermicro's 2026 Blackwell/Blackwell Ultra
  systems were found in collected sources; Supermicro sells via quote/channel
  **[unverified]** — pricing information is not publicly disclosed. Flagged as a gap.
- Deal-level figures are vendor/press-reported rather than list prices (see 1.4).

### 1.4 Major customer wins / partnerships (2026 and relevant 2025 context)

- **Cisco Secure AI Factory with NVIDIA** (Aug 2026): Supermicro AI systems sold via
  Cisco's enterprise and AI-cloud partners from Oct 2026 — no disclosed dollar value
  **[official/independent]** (see 1.2).
- **SK Telecom + Schneider Electric** (MWC, Mar 2026): modular AI data-center alliance
  **[secondary]**.
- **Lambda + ECL** (announced Sept 2025, still the reference 2026 GB300 deployment):
  first hydrogen-powered production-grade **NVIDIA GB300 NVL72** systems at ECL's
  Mountain View MV1 off-grid campus — **Supermicro-built, 142 kW systems**, ~4,000 lbs
  each, direct-to-chip liquid cooling, integrated into production in under two hours;
  Lambda doubled its footprint to 100% of MV1 **[independent]** (convergedigest.com;
  globalhydrogenreview.com). Supermicro SVP Technology & AI Vik Malyala confirmed the
  systems were delivered by Supermicro **[official quote]**.
- **DataVolt** (announced May 2025): **$20 billion partnership** to build hyperscale AI
  campuses in Saudi Arabia using Supermicro DLC-2 liquid-cooled systems
  **[secondary (ainvest.com)]** — the headline figure comes from market commentary, not
  a verified vendor filing; treat as **[unverified]** pending primary-source
  confirmation.
- **xAI (Colossus, Memphis)**: 2024–2025 context — Colossus supercomputer built with
  both **Dell and Supermicro** servers, ~100,000 NVIDIA GPUs, expanded toward 200,000
  **[secondary (SDxCentral)]**.
- CoreWeave/Nscale: **no specific 2026 Supermicro–CoreWeave win found** in collected
  sources — flagged **[unverified]**; not claimed.

### 1.5 AI revenue figures (FY2026 / quarterly)

All FY2026 figures below are **[vendor-reported]** unless noted.

- **Q3 FY2026** (reported ~May 5, 2026): revenue **$10.24B, +123% YoY**; EPS **$0.84** vs
  ~$0.62 consensus **[secondary (ainvest.com, June 2026)]**.
- **Q4 FY2026 preliminary update** (July 21, 2026, Business Wire, **[official]**):
  revenue estimated **near the low end of $11.0–12.5B guidance**; GAAP and non-GAAP
  gross margins estimated **15–17% vs 8.2–8.4% guided**, "primarily due to a favorable
  customer and product mix"; **backlog at record levels**, with **new orders in excess
  of $60B received during Q4 FY2026**, deliverable over future quarters.
- **Q4 FY2026 actual** (reported Aug 11, 2026): net sales **$11.1B (≈$11.12B), +93% YoY**
  vs ~$11.55B consensus (miss); non-GAAP diluted EPS **$1.70** vs $0.94 estimate (beat);
  GAAP diluted EPS **$1.62** vs $0.53–0.67 guided; GAAP gross margin **17.5%** (rebounded
  from 9.9% in Q3 FY2026, +760 bps sequentially); operating cash flow **+$747M**
  (vs -$6.6B in Q3 FY2026); FCF **+$719M** **[secondary (bingx.com; r40.io)]**.
- **Full FY2026** (ended June 30, 2026): net sales **$39.06–39.1B, +78% YoY** (vs $22.0B
  FY2025); GAAP net income **$2.2–2.23B** (vs $1.0B); GAAP EPS **$3.26**
  **[secondary (bingx.com; r40.io)]**. Full-year operating cash flow was **-$6.81B**
  (vs +$2.23B net income), driven by **-$8.88B inventory** and **-$3.92B receivables**;
  full-year FCF **-$6.97B**, financed with **$4.47B of loans, $1.41B of common stock,
  and $4.23B of Mandatory Convertible Preferred** — hence $13.1M preferred dividends
  and guided diluted share count rising to ~745M **[secondary (r40.io)]**.
- **Q1 FY2027 guidance** (Aug 2026): revenue **$14.5–15.5B** (vs $11.8B FactSet
  consensus); GAAP EPS **$0.89–0.98** on ~745M diluted shares — implying net income
  ~$663–730M, roughly half of Q4 FY2026's $1,178M despite +30–39% higher revenue
  **[secondary (bingx.com; r40.io)]**.
- **FY2027 outlook**: revenue **$65–72B** (midpoint $68.5B) vs ~$52.5B Wall Street
  estimate **[secondary (bingx.com)]**.
- Historical guidance datapoints: Feb 11, 2026 update projected **$40B FY2026 revenue**
  (+~70% YoY), called "conservative" by CEO Charles Liang **[secondary
  (financialmodelingprep)]** — actual came in ~$39.1B **[vendor-reported]**; Nov 4,
  2025 disclosure of **"more than $13B in Blackwell Ultra orders"** and raised FY2026
  target of "at least $36B" **[secondary (globenewswire.com investor-alert text)]**;
  Oct 23, 2025 guidance reiteration of "at least $33B for FY2026" **[secondary]**.

### 1.6 Accounting / governance status updates (Nasdaq listing, auditor, filings)

- **Background (2024)**: Ernst & Young (EY) resigned as auditor in late October 2024,
  citing concerns over internal controls, board independence, and accounting practices
  **[independent (Bloomberg Law; ainvest.com)]**; this followed Hindenburg Research's
  August 2024 short-seller report alleging accounting irregularities, undisclosed
  related-party transactions, and potential export-control violations **[independent]**.
  Supermicro delayed its FY2024 10-K and Q1 FY2025 10-Q; Nasdaq notified the company of
  non-compliance (Sept 2024) with a 60-day window to submit a compliance plan or risk
  delisting **[independent]**.
- **BDO appointment (Nov 2024)**: the board's audit committee appointed **BDO USA, P.C.**
  as independent auditor, effective immediately; a Nasdaq compliance plan was submitted
  **[independent (Stocktwits, Nov 2024; DIGITIMES)]**; stock jumped ~30–32% on the news
  **[independent]**.
- **Special committee (2024)**: an independent special committee investigated the
  matters raised by EY and reported no significant concerns regarding senior
  management's integrity or financial accuracy; committee work completed with other
  ongoing items remaining **[independent]**.
- **Compliance restored (Feb 2025)**: Supermicro filed its delayed **10-K (FY ended
  June 30, 2024)** and **10-Qs (Q1 FY2025 ended Sept 30, 2024; Q2 FY2025 ended Dec 31,
  2024)**, resolving Nasdaq listing non-compliance; shares jumped ~17% after hours
  **[secondary (financialmodelingprep)]**.
- **Annual meeting (April 15, 2026)**: stockholders ratified **BDO USA, P.C. as auditor
  for the fiscal year ending June 30, 2026**; elected **Charles Liang, Tally Liu, and
  Sherman Tuan** as Class I directors; approved an amended 2020 Equity and Incentive
  Compensation Plan authorizing **118,000,000 shares** (103M previously + 15M new), and
  approved a non-binding advisory vote on executive pay **[independent (stocktitan.net
  8-K summary, 8-K dated April 20, 2026)]**.
- As of Sept 22, 2026, **no new auditor change or Nasdaq non-compliance notice** was
  found in collected sources — flagged **[unverified]** (absence of evidence, not
  evidence of absence).

### 1.7 2026 controversies / investigations updates

This is the dominant 2026 governance story for Supermicro — the 2024 accounting crisis
was resolved, but a new **DOJ export-control indictment** landed in March 2026:

- **March 19, 2026 — DOJ indictment unsealed** **[independent (Pomerantz class-action
  complaint, filed 03/25/26; globenewswire.com investor alert; ainvest.com)]**: three
  individuals associated with Supermicro were charged with conspiring to divert
  **~$2.5 billion worth of servers containing advanced NVIDIA GPUs** to customers in
  China (2024–2025) without required export licenses. The indicted individuals:
  Supermicro's **co-founder and Senior Vice President of Business Development**, a
  **general manager in Supermicro's Taiwan office**, and a **third-party broker**
  **[independent]**. Supermicro itself was **NOT named as a defendant** and is not
  accused of wrongdoing **[independent (financialcontent.com, Aug 20 2026
  announcement)]**.
- **Market reaction**: stock fell **~27%** in a single session on March 20, 2026
  (ainvest cites ~27%; Hagens Berman alert cites a 33.3% drop) — figures differ by
  source **[secondary/independent]**; market cap fell from a 2024 AI-boom peak of
  ~$67B to ~$18.5B post-indictment, recovering only to ~$23.3B by Aug 20, 2026
  **[secondary (ainvest.com)]**.
- **April 7, 2026 — independent board investigation announced** **[independent
  (Barchart, Apr 9, 2026)]**: led by **Scott Angel** (Lead Independent Director,
  ~40-year Deloitte veteran including 25 years as audit partner) and **Tally Liu**
  (Audit Committee Chair, CPA); retained **Munger, Tolles & Olson LLP** and forensic
  accounting firm **AlixPartners**; findings reported directly to independent board
  members, not management. CEO Charles Liang stated in a stakeholder letter that
  Supermicro was "a victim of the elaborate schemes orchestrated by these individuals,
  which deceived both federal authorities and our internal compliance team"; on the
  early-May Q3 FY2026 earnings call he insisted "no one" beyond the indicted employees
  was involved and that the company is not a target of the investigation
  **[secondary (ainvest.com, June 2026)]**.
- **August 20, 2026 — investigation completed** **[official (Supermicro press release
  via markets.financialcontent.com, bizwire-2026-8-20)]**: the independent board
  investigation found **no evidence that "any current member of senior management" had
  knowledge** of the alleged diversion scheme; the three individuals "no longer have
  any relationship with Supermicro"; the company announced continued enhancement of its
  export-compliance program. Market reaction was negative: shares fell ~1.6–2% on the
  day **[secondary (ainvest.com, Aug 2026)]**.
- **Securities class actions** (ongoing as of report date): Pomerantz complaint filed
  **March 25, 2026** (Case 3:26-cv-02606) under the Exchange Act of 1934, class period
  **Feb 2, 2024 – Mar 19, 2026** **[independent (Pomerantz complaint PDF)]**; Hagens
  Berman filed a new class action (announced ~May 2026), lead-plaintiff deadline
  **May 26, 2026** **[secondary (ainvest.com)]**.
- **DOJ/SEC investigation status**: the indictment named individuals only; whether any
  corporate-level DOJ or SEC enforcement action against Supermicro itself remains open
  was **not established** in collected sources — flagged **[unverified]**. Liang's May
  2026 statement that the company "is not a target" is a **[vendor-reported]** claim,
  not an independent confirmation.

---

## PART 2 — DELL TECHNOLOGIES (NYSE: DELL)

### 2.1 PowerEdge AI servers 2026 — portfolio and launch dates

Dell's AI server portfolio in 2026 centers on the **Dell AI Factory with NVIDIA** and
the PowerEdge XE series **[official]**:

- **PowerEdge XE9680** — Dell's "fastest ramping solution ever" (predecessor to the
  2026 XE generation); 8-GPU platform supporting NVIDIA H100/H200/B200-class GPUs
  **[official]**.
- **PowerEdge XE9780 and XE9785** (air-cooled) + **XE9780L and XE9785L** (liquid-cooled),
  announced as the next-generation XE line with NVIDIA: successors to the XE9680,
  supporting **8-way NVIDIA HGX B300** and delivering **up to 4× faster LLM training**
  vs XE9680; configurations support **up to 192 NVIDIA Blackwell Ultra GPUs** with
  direct-to-chip liquid cooling, customizable to **up to 256 Blackwell Ultra GPUs per
  Dell IR7000 rack** **[official (Dell announcement via TechPowerUp techpowerup.com/336994;
  smetechguru.co.za)]**. The announcement (Dell Technologies World 2025 cycle) also
  introduced: **PowerEdge XE9712 with NVIDIA GB300 NVL72** (rack-scale; **50× more AI
  reasoning inference output, 5× throughput improvement**; new Dell PowerCool
  technology) **[official]**; **PowerEdge XE7745 with NVIDIA RTX PRO 6000 Blackwell
  Server Edition** (available July 2025, 4U, up to 8 GPUs, for physical/agentic AI —
  robotics, digital twins, multimodal) **[official]**; planned support for the **NVIDIA
  Vera CPU** and a new Dell PowerEdge XE server for **Vera Rubin** (Dell Integrated
  Rack Scalable Systems) **[official]**; networking: Dell PowerSwitch SN5600/SN2201
  (Spectrum-X Ethernet) and NVIDIA Quantum-X800 InfiniBand (800 Gb/s)
  **[official]**.
- **PowerEdge XE9785 / XE9785L with AMD Instinct MI355X** (announced at **SC25, St.
  Louis — November 2025**, per HotHardware): dual AMD EPYC CPUs + **8× AMD Instinct
  MI355X per node**; liquid-cooled variant is **3U per node** → **128 MI355X GPUs per
  standard 42U rack**; each MI355X has **288 GB HBM3e**, FP6/FP4 support, ~5 FP8
  PFLOPS per GPU (~40 PFLOPS per air-cooled 10U node); AMD Pensando Pollara 400 NICs;
  Dell PowerSwitch AI fabric (new **PowerSwitch Z9964F-ON / Z9964FL-ON**, liquid-cooled
  networking variant); also **PowerEdge R770AP** with Intel Xeon 6 6900-series
  (up to 128 P-cores per socket, dual socket, 12-channel memory), configurable with
  Instinct accelerators **[independent (HotHardware, Nov 2025)]**. ⚠️ Date note: the
  HotHardware article is from SC25 (Nov 2025), not 2026 — it is included because it
  defines Dell's 2026 AMD platform lineup.
- **PowerEdge XE9680L** (liquid-cooled 4U): supports **8× NVIDIA Blackwell GPUs**;
  "highest possible rack-scale density for NVIDIA GPUs in an industry standard x86
  rack," **33% more GPU density per node**, 20% more PCIe Gen5 slots, double
  North/South network expansion capacity; direct liquid cooling; factory-integrated
  rack-scale deployment **[official (Dell AI Factory update via
  digitalisationworld.com, updated ~June 2026)]**. ⚠️ The XE9680L was first announced
  at Dell Tech World 2024 (May 2024); the June-2026 item may be a re-announcement or
  availability update — flagged **[unverified]** which 2026 event it belongs to.
- **Turnkey rack-scale variants** (announced alongside XE9680L): air-cooled design
  supporting **64 GPUs in a single rack**, or liquid-cooled format with **72 NVIDIA
  Blackwell GPUs in a single rack** **[official]**.
- Dell AI Data Platform / Dell NativeEdge / Generative AI Solutions for Digital
  Assistants updates were announced alongside (non-server software) **[official]**.

### 2.2 xAI / Colossus-related deals

- **Feb 14–15, 2025 — Bloomberg/Reuters report** **[independent]**: Dell was "nearing a
  deal **worth more than $5 billion**" to provide xAI with AI-optimized servers
  containing **NVIDIA GB200** semiconductors, for delivery "this year" (2025); details
  still being discussed and subject to change; Dell and xAI did not comment
  (reuters.com via wdsm710.com; d2649.cms.socastsrm.com).
- **Margins**: a Bloomberg follow-up reported the $5B xAI deal would yield
  **mid-single-digit gross margins**, with **NVIDIA providing the networking** (a
  category Dell also sells); interviewed workers at the server makers described
  Nvidia's demands as overbearing **[independent (Bloomberg via visive.ai)]**.
- **Prior Colossus build**: the Memphis **Colossus** supercomputer was built using both
  **Dell and Supermicro** servers, ~100,000 NVIDIA GPUs; Musk planned expansion to
  200,000 and then stated a 1M-GPU target **[secondary (SDxCentral; en.tmtpost.com)]**.
  Dell COO Jeff Clarke told Bloomberg (Dec 2025) the company had **shipped tens of
  thousands of GPUs to xAI's supercomputer** **[secondary (en.tmtpost.com)]**.
- **Whether the $5B deal was finalized and how much has shipped in 2026: NOT publicly
  confirmed** in collected sources — flagged **[unverified]**. No Dell press release or
  2026 earnings statement names xAI as a completed $5B contract. Parent should verify
  before asserting the deal closed.

### 2.3 AI server revenue / backlog figures from earnings (ISG AI orders)

All figures **[vendor-reported]** (Dell official Q-results press releases):

- **Q4 FY2026** (reported Feb 26, 2026, quarter ended Jan 30, 2026): revenue **$33.4B,
  +39% YoY** (record); non-GAAP diluted EPS **$3.89, +45%**; **AI orders $34.1B** (record);
  **AI server revenue $9.0–9.5B, +342% YoY** (record; sources cite $9.0B and $9.5B —
  minor discrepancy between the official release and secondary reporting, flagged);
  **AI backlog $43.0B exiting Q4** (record); ISG revenue $19.6B +73%, operating income
  $2.9B +41%. **Full FY2026**: revenue **$113.5B, +19%** (record); non-GAAP EPS
  **$10.30, +27%**; **AI orders >$64B ($64.1B)**; **AI shipments $25.2B**; ISG
  **$60.8B, +40%**, operating income **$7.1B, +27%**; cash flow from operations
  **$11.2B**; capital returned to shareholders $7.5B; dividend raised 20%, buyback
  authorization +$10B. Initial FY27 guidance: full-year revenue **$140B** midpoint,
  **AI-optimized server revenue $50B** (>100% growth).
- **Q1 FY2027** (reported May 28, 2026): revenue **$43.8B, +88% YoY** (record); diluted
  EPS $5.24 +282%, non-GAAP EPS **$4.86, +214%**; **AI orders $24.4B**; **AI server
  revenue $16.1B, +757% YoY** (record); **AI backlog $51.3B** (record, per Zacks);
  ISG **$29.0B, +181%**; operating income $3.1B +206%. Raised FY27 revenue guidance to
  **$167B midpoint** (~+50% YoY), **AI server revenue to $60B**. Clarke: AI customer
  count topped **5,000** (+50% in six months) **[secondary (Zacks, May 29, 2026)]**;
  cited DRAM/NAND/CPU supply bottlenecks, 3–5-year customer supply deals
  **[secondary]**.
- **Q2 FY2027** (reported Sep 1, 2026, quarter ended Jul 31, 2026): revenue
  **$47.0B, +58% YoY** (record); GAAP EPS $6.34 +273%, non-GAAP EPS **$7.04, +203%**;
  **AI orders $60.9B** (record, "the most in our history" — Clarke); **AI server
  revenue $16.4B, +100% YoY** (record); **AI backlog $95B** (record, ~8× YoY per
  techx.pk); **H1 FY2027 AI revenue $32.5B** (vs $10.1B prior-year period — more than
  tripled); **trailing-12-month AI orders $131.7B** **[secondary]**; AI customers
  **>6,500** (+3,300 in the last three quarters); ISG **$31.8B, +89%**, operating
  income **$4.8B, +225%**; Traditional Servers & Networking **$10.5B, +122%**; Storage
  **$4.9B, +26%**; CSG **$15.0B, +20%**. Raised FY27 revenue guidance to **$192B**,
  non-GAAP EPS to **$25.50**, **AI server revenue to $74B** **[official (Dell press
  release via morningstar.com, bizwire-20260901)]**. Supporting stock news: $5B
  investment-grade bond offering drew ~$23B of orders; Dell added to the **S&P 100**
  **[secondary (tickeron.com)]**.
- Analyst price-target wave post-Q2: $635 JPMorgan, $600 Citi/Mizuho, $640 RBC
  Outperform initiation **[secondary]** — market commentary, not vendor guidance.

### 2.4 Partnerships (NVIDIA, CoreWeave, etc.)

- **NVIDIA**: Dell AI Factory with NVIDIA — co-developed PowerEdge XE9780/XE9785/XE9712
  line, NVIDIA Enterprise AI Factory validated design, Spectrum-X/Quantum-X800
  networking, Dell ProSupport **[official]**; Dell plans support for NVIDIA Vera CPU
  and Vera Rubin platform **[official]**.
- **AI-factory ecosystem (Q1 FY27 earnings call, May 2026)**: Clarke named **NVIDIA,
  Google Cloud, OpenAI, ServiceNow, and Palantir** as AI-factory ecosystem partners,
  with offerings spanning data-center infrastructure to deskside AI systems
  **[vendor-reported via secondary (Zacks, May 29, 2026)]**.
- **AMD**: MI355X-based XE9785/XE9785L **[independent (HotHardware)]**; prior MI300X
  XE9680 program **[official]**.
- **CoreWeave**: **no CoreWeave–Dell 2026 deal found** in collected sources — flagged
  **[unverified]**; not claimed. (Note: a Supermicro–Cisco Secure AI Factory win is
  the comparable 2026 channel win — see 1.4.)
- **ServiceNow**: Digital Assistants GenAI solution **[official]**.

### 2.5 Pricing

- **No published per-server list prices** found for Dell's 2026 PowerEdge XE
  (XE9680/XE9780/XE9785/XE9712) lineup in collected sources — flagged as a gap
  **[unverified]**.

---

## CROSS-CUTTING NOTES / EXPLICIT UNCERTAINTIES

1. **Prices**: neither vendor publishes list prices for 2026 Blackwell-class servers;
   treat all deal values as reported-only.
2. **Dell xAI $5B deal finalization**: the Feb-2025 Bloomberg/Reuters report says
   "nearing"/"close to finalizing"; no primary source found confirming it closed or
   quantifying 2026 shipments. **[unverified]**
3. **Dell Q4 FY26 AI revenue**: official release says $9.0B (+342%); Zacks says $9.5B
   shipped; both are cited — discrepancy flagged.
4. **Supermicro HGX B300 4U/2OU announcement date**: stocktitan page is undated; page
   metadata suggests ~Nov 2025. Treated as "~Oct–Nov 2025." **[unverified date]**
5. **Supermicro Blackwell Ultra (HGX B300 NVL16 / GB300 NVL72) announcement date**: the
   PR text's article slug reads "2025-03-19" (GTC 2025 timing), though collected
   alongside 2026 items; date treated as uncertain. **[unverified date]**
6. **NVIDIA GTC Washington, D.C.** (Supermicro Vera Rubin NVL144/CPX 2026 plans):
   announcement text has no explicit year in the PR excerpt; GTC DC 2025 was Oct 27–29,
   2025 — placed as ~Oct 2025 with uncertainty flag. **[unverified date]**
7. **Dell XE9785 MI355X launch**: attributed to SC25 (Nov 2025), not 2026 — included
   for 2026 lineup completeness.
8. **Dell XE9680L June-2026 item**: first announced May 2024; the 2026-dated article
   may be an update/re-announcement — do not treat as a new 2026 launch without
   verification. **[unverified]**
9. **Supermicro DOJ/SEC corporate liability**: indictment covers three individuals;
   company not named; corporate-level exposure and class-action outcomes remain
   **[unverified]** as of 2026-09-22.
10. **Supermicro Q2 FY2026 (Feb 2026) details** were not pulled individually; the
    FY2026 totals and Q3/Q4 datapoints above cover the trajectory. Minor gap.
11. **CoreWeave partnerships for either vendor in 2026**: not found — **[unverified]**,
    do not assert.
12. The **$20B DataVolt** figure is from market commentary (ainvest.com), not a vendor
    filing — **[unverified]**.

---

## SOURCES (verbatim URLs)

### Supermicro
- https://www.businesswire.com/news/home/20260721109492/en/Supermicro-Provides-Fourth-Quarter-of-Fiscal-Year-2026-Preliminary-Business-Update
- https://bingx.com/en/learn/article/super-micro-computer-smci-stock-forecast-and-price-prediction-is-smci-a-good-investment
- https://r40.io/news/smci-q4-fy2026-earnings-analysis/
- https://www.stocktitan.net/news/SMCI/supermicro-expands-nvidia-blackwell-portfolio-with-new-4u-and-2-ou-o941gt6bwal2.html
- https://www.stocktitan.net/news/SMCI/supermicro-expands-collaboration-with-nvidia-and-strengthens-80ndque6dmqu.html
- https://www.stocktitan.net/sec-filings/SMCI/8-k-super-micro-computer-inc-reports-material-event-f320b49172cb.html
- https://www.techPowerUp.com/342349/supermicro-expands-nvidia-collaboration-focuses-on-u-s-made-ai-systems-for-government-use
- https://barchart.com/story/news/35737389/supermicro-expands-collaboration-with-nvidia-and-strengthens-compliance-data-integrity-and-quality-of-u-s-based-manufacturing-of-ai-infrastructure-solutions-optimized-for-government-applications
- https://www.europeansuntimes.com/news/supermicro-adds-portfolio-for-next-wave-of-ai-with-nvidia-blackwell-ultra-solutions-featuring-nvidia-hgxtm-b300-nvl16-and-gb300-nvl7220250319113647//
- https://learn-more.supermicro.com/data-center-stories/supermicro-cisco-secure-ai-factory-nvidia
- https://equityswarm.com/stocks/company/smci/smci-partners-with-cisco-to-embed-ai-servers-in-cisco-secure-ai-factory/
- https://www.foreignpolicyjournal.com/2026/08/27/super-micro-computer-nasdaq-smci-deepens-ai-data-center-push-with-cisco-and-nvidia-partnership/
- https://convergedigest.com/lambda-and-ecl-power-up-first-hydrogen-fueled-nvidia-gb300-nvl72-systems/
- https://www.globalhydrogenreview.com/hydrogen/24092025/lambda-and-ecl-launch-nvidia-gb300-nvl72-online/
- https://www.ainvest.com/news/supermicro-20b-ai-campus-deal-catalyst-hyperscale-dominance-sustainable-tech-leadership-2505/
- https://business.dailytimesleader.com/dailytimesleader/article/barchart-2026-3-5-as-supermicro-joins-a-key-ai-data-center-alliance-should-you-buy-smci-stock
- https://greencitylivestock.com/news/story/36864445/super-micro-computer-just-launched-a-new-ai-server-should-you-buy-smci-stock-today
- https://www.Digitimes.Com/news/a20241119PD208/supermicro-ai-server-delisting-nasdaq.html
- https://news.bloomberglaw.com/esg/super-micro-shares-jump-on-compliance-plan-new-auditor
- https://site.financialmodelingprep.com/market-news/supermicro-nasdaqsmci-soars--after-filing-delayed-financial-reports
- https://www.ainvest.com/news/super-micro-shares-surge-30-new-auditor-boosts-nasdaq-compliance-hopes-241110102592ccbbdd5a6512/
- https://www.ainvest.com/news/current-supermicro-clear-percent-market-charged-2608/
- https://www.ainvest.com/news/super-micro-survived-doj-crisis-financials-harder-story-2606/
- https://www.ainvest.com/news/securities-class-action-filed-super-micro-computer-executives-alleging-export-violations-concealed-information-2605/
- https://www.globenewswire.com/news-release/2026/05/05/3287309/3080/en/SMCI-Investor-Alert-Super-Micro-Computer-Securities-Fraud-Lawsuit-Investors-With-Losses-May-Seek-to-Lead-the-Class-Action-After-Allegedly-Misrepresenting-Revenue-Derived-from-Sales.html
- https://pomerantz.nyc3.digitaloceanspaces.com/complaints/SMCI/Super Micro Computer, Inc. - Complaint.pdf
- https://markets.financialcontent.com/fatpitch.valueinvestingnews/article/bizwire-2026-8-20-supermicro-announces-completion-of-independent-investigation-and-continued-enhancement-of-export-compliance-program
- https://markets.financialcontent.com/gatehouse.rrstar/article/barchart-2026-4-9-super-micro-computer-is-auditing-china-allegations-itself-does-that-make-smci-stock-a-buy

### Dell
- https://www.dell.com/en-us/dt/corporate/newsroom/announcements/detailpage.press-releases~usa~2026~2~dell-technologies-delivers-fourth-quarter-and-full-year-fiscal-2026-results.htm
- https://www.dell.com/en-us/dt/corporate/newsroom/announcements/detailpage.press-releases~usa~2026~05~dell-technologies-delivers-first-quarter-fiscal-2027-financial-results.htm
- https://www.morningstar.com/news/business-wire/20260901574850/dell-technologies-delivers-second-quarter-fiscal-2027-financial-results
- https://WWW.ZACKS.COM/stock/news/2877003/dell-q4-earnings-beat-estimates-revenues-rise-yy-shares-up?art_rec=blog-analyst_blog-up_next-ID01-img-2877003
- https://www.blocksandfiles.com/flash/2026/02/27/ai-server-frenzy-fuels-record-revenues-for-dell/4092727
- https://www.zacks.com/stock/news/2928734/dells-q1-earnings-call-centers-on-ai-supply-and-demand
- https://pulse2.com/dell-technologies-q2-revenue-hits-record-47-billion-as-ai-server-backlog-reaches-95-billion-and-fy27-outlook-rises-to-192-billion/
- https://sharesify.com/dell-q2-fy2027-ai-demand-surges-but-expectations-are-now-sky-high/
- https://techx.pk/dell-q2-fy2027-ai-server-orders-backlog-95-billion/
- https://finance.biggo.com/news/US_DELL_2026-09-08
- https://finance.biggo.com/news/US_DELL_2026-05-28
- https://www.techpowerup.com/336994/dell-technologies-unveils-next-generation-enterprise-ai-solutions-with-nvidia
- https://www.smetechguru.co.za/dell-technologies-unveils-next-generation-enterprise-ai-solutions-with-nvidia/
- https://hothardware.com/news/dell-announces-poweredge-ai-servers
- https://digitalisationworld.com/news/67716/dell-technologies-expands-dell-ai-factory
- https://www.storagenewsletter.com/2024/05/27/dell-technologies-world-expanded-dell-ai-factory-with-nvidia-to-turbocharge-ai-adoption/
- https://d2649.cms.socastsrm.com/2025/02/14/dell-nears-deal-to-sell-5-billion-in-ai-servers-to-xai-bloomberg-news-reports/
- https://www.sdxcentral.com/news/musks-xai-considering-second-data-center-5bn-dell-chip-deal/
- https://www.brandiconimage.com/2025/02/dell-close-to-finalizing-5-billion-deal.html
- https://en.tmtpost.com/post/7462003
- https://www.mitrade.com/insights/news/live-news/article-3-641092-20250215

*End of report — compiled 2026-09-22. Work was read-only; nothing sent externally.*


---

## §2 HPE + Giga Computing + ASUS + MSI + xFusion

# Server Vendors — Wave 2: HPE + Giga Computing (Gigabyte) + ASUS + MSI + xFusion
**As of: September 22, 2026** | **RAG collection draft — Step 5 (volet 1, etape 5)**

> Provenance legend used on every factual claim:
> `[official]` = vendor's own press release / datasheet / store page
> `[vendor-reported]` = vendor disclosure via earnings deck, call, or investor filing (management commentary)
> `[independent]` = reputable independent outlet (ServeTheHome, The Register, IDC, MLCommons, etc.)
> `[secondary]` = press/analyst/blog aggregation of primary sources
> `[unverified]` = low-confidence claim, single weak source, or gap explicitly flagged

---

## 1. HPE (Hewlett Packard Enterprise)

### 1.1 AI server lineup (2026)

**HPE ProLiant Compute XD685 (8-GPU node)**
- Support for **8× NVIDIA Blackwell Ultra (B300 HGX)**, **8× NVIDIA Blackwell (B200)**, **8× NVIDIA H200 Tensor Core**, **or 8× AMD Instinct MI355X** accelerators [official — HPE Store UK product page, SKU #S4Q28A, https://buy.hpe.com/uk/en/compute/rack-servers/proliant-compute-xd600-servers/proliant-compute-xd600-server/hpe-proliant-compute-xd685-air-cooling-server/p/s4q28a].
- Direct liquid cooling (DLC) available for **all** GPU options; air cooling available for H200 configurations only [official — same HPE store page].
- Modular 5U chassis for DLC environments / 6U chassis for air-cooled configurations; 2× 5th Gen AMD EPYC processors (up to 5.0 GHz, 32–160 cores) [official — HPE datasheet].
- 24× DDR5-6400 RDIMM slots, 12 memory channels per CPU, ECC; HPE iLO 6 management with silicon root of trust / Zero Trust [official — HPE store page].
- The XD685 was originally announced for 5th Gen AMD EPYC AI training workloads (announcement covered by VarIndia) [secondary — https://varindia.com/news/hpe-unveils-new-solutions-to-boost-ai-model-training].
- ⚠️ **[unverified/gap]**: The task mentions **ProLiant Compute XD680**; the official HPE store source consulted lists only the XD685 with B300/B200/H200/MI355X options. The Register (Nov 2024) previously reported an XD680 variant configured with 8× Intel Gaudi3 accelerators [independent — The Register, https://www.theregister.com/on-prem/2024/11/13/hpe-crams-224-nvidia-blackwell-gpus-into-latest-cray-ex/]. Whether XD680 remains a live 2026 SKU with updated GPU options was not confirmed — flagged as a gap.

**HPE Cray XD670 (HPC/AI, Intel CPU base)**
- 5U chassis, single node; **8× NVIDIA H200 SXM (700 W TDP, 141 GB HBM3e each)** or H100; 2× 5th Gen Intel Xeon Scalable (up to 400 W TDP); 32× DDR5 DIMM slots (8-channel/socket, up to 5600 MT/s); PCIe Gen5; optional plug-and-play **direct liquid cooling**; integrated storage [official — HPE Cray XD670 datasheet, https://www.hpe.com/psnow/generateDDS/HPE%20Cray%20XD670%20data%20sheet-PSN1014737116TREN.pdf].
- MLPerf Inference v5.0 top rankings cited in Subaru win [vendor-reported].

**HPE Cray XD675 (Blackwell)** ⚠️ **[unverified/gap]**: The task brief lists Cray XD675 with NVIDIA B200, but the research surfaced no 2026-dated primary source confirming an XD675 SKU. The Register (Nov 2024) described Cray EX Blackwell blades (224 GPUs per cabinet, shipping late 2025) [independent], not an XD675. Treat XD675 as an unconfirmed product name.

**Liquid cooling** — HPE markets decades of liquid-cooling deployment leadership; DLC options span XD685 (all GPU options), Cray XD670, and rack-scale solutions [official].

### 1.2 Juniper Networks acquisition — status as of Sept 2026

- Deal **closed July 2, 2025**; Juniper stock (JNPR) ceased trading on NYSE and the company was integrated into HPE [secondary — https://www.levelfields.ai/news/hpe-finalizes-14-billion-juniper-networks-acquisition; https://channellife.ca/story/hpe-completes-juniper-networks-deal-to-boost-ai-cloud-focus]. Deal value widely cited as **~$14 billion** [secondary — multiple outlets].
- **August 2026 legal milestone**: U.S. District Judge Casey Pitts dismissed a challenge by a Colorado-led coalition of state attorneys general against the DOJ settlement, issuing a 41-page ruling finding the states had not shown the amended final judgment was against the public interest. This gave HPE final court approval to operate as an integrated entity [secondary — https://www.ainvest.com/news/hpe-clears-juniper-deal-ai-hardware-stocks-rally-2608/]. Date calibration: the article was "last updated 39 days ago" on ~Sept 22, 2026 → ruling **mid-August 2026**.
- DOJ settlement terms (agreed 2025): HPE **divested its Instant On wireless networking business** and **auctioned a license for Juniper's Mist AI software** [secondary — ainvest article above].
- Integration: former Juniper CEO **Rami Rahim** leads the combined **HPE Networking** business unit (Juniper portfolio + HPE Aruba); networking business doubled in size; networking now accounts for **over 50% of HPE's total operating income** [secondary — levelfields]. HPE says integration is **ahead of plan**, targeting **$600M annualized cost synergies by end of fiscal 2028**; net leverage fell to **1.8×** (below the 2× target) more than a year early [secondary — Zacks, https://www.zacks.com/stock/news/2984471/hpe-q3-earnings-surpass-expectations-revenues-rise-yy].
- **AI networking implications**: "Networks for AI" orders of **$0.7B in Q3 FY2026** (quarter ended July 31, 2026), **$2.2B cumulative**, with HPE raising its fiscal-2026 target to **$2.5–$3B** [vendor-reported via Zacks — https://www.zacks.com/stock/news/2984471/hpe-q3-earnings-surpass-expectations-revenues-rise-yy]. Reported networking revenue was $2.89B (+74.9% YoY as reported, including Juniper; **+10% normalized** for the acquisition) — the two figures must not be conflated [vendor-reported via fourweekmba/payney analyses].

### 1.3 AI customer wins (2026)
- **$3.5 billion inferencing deal with an unnamed hyperscaler**, awarded **after quarter-end** (i.e., August 2026), disclosed on HPE's Q3 FY2026 earnings deck and call [vendor-reported — HPE Q3 FY26 earnings slides, https://files.quartr.com/conference-calls/cdd89295c3957b4612350653f2c0618f-2026-09-02-20-15-50.pdf].
- **Oracle gigawatt-scale networking deal**: expanded arrangement for routers/switches supporting Oracle's AI data center buildout, announced Sept 2, 2026; includes **HPE warrants issued to Oracle** — share count, strike price, conditions, and dilution all **undisclosed** [secondary — SiliconANGLE, https://siliconangle.com/2026/09/02/ai-demand-lifts-hpe-and-netapp-to-record-quarters-investors-sell-both/].
- **Subaru (2025 win, still in-market)**: HPE Cray XD670 with NVIDIA H200 GPUs for next-gen EyeSight AI driver-assist development (image recognition training/inference), on-prem deployment + HPE Tech Care; announced **July 28, 2025** [official — Business Wire via StockTitan, https://www.stocktitan.net/news/HPE/subaru-selects-hpe-to-accelerate-ai-development-for-next-generation-85xppynm4hze.html].

### 1.4 AI revenue figures — Q3 FY2026 earnings (reported Sept 2, 2026; quarter ended July 31, 2026)
- Total revenue **$12.2B**, +34% YoY (above high end of guidance); non-GAAP diluted EPS **$1.11**; non-GAAP gross margin record **40.4%** [vendor-reported — press release + SEC 8-K, via https://fourweekmba.com/ai-hpe-q3-fy2026-ai-supply-constraint-memory-bottleneck/].
- **AI systems orders: $2.4B** in the quarter, up **>30% sequentially**; **AI systems backlog: $6.8B** (+14% sequentially) [vendor-reported — HPE earnings deck via Zacks, https://www.zacks.com/stock/news/2984417/hpe-q3-earnings-call-highlights-ai-demand-networking-shift].
- **Total AI backlog (AI Systems + Networks for AI): record $7.6B** exiting the quarter [vendor-reported — same sources; also https://payney.com/blog/hpe-q3-2026-122b-revenue-fy26-eps-outlook-raised/].
- Segment: **Cloud & AI revenue $9.04B, +25.4% YoY**, operating margin 17%; **server revenue $6.77B, +35.3%**; traditional server orders +75% YoY (AI-ready configs, higher ASPs); Private Cloud AI orders up **triple digits** YoY [vendor-reported — Zacks, https://www.zacks.com/stock/news/2987783/hpe-surges-26-in-3-months-is-it-the-right-time-to-buy-the-stock].
- Supply constraint: management attributed the gap between normalized orders (+42%) and revenue (+34%) to **DDR5, DDR4, NAND, HBM, and wafer capacity** — expected to persist into fiscal 2027 [vendor-reported].
- Guidance: FY2026 revenue growth raised to **34–37%** (~$46.5B midpoint), non-GAAP EPS **$3.75–$3.85**, free cash flow ≥**$3.75B**; Q4 FY26 revenue guided **$13.9–$14.8B**; FY2027 framework raised to **13–17% revenue growth**, EPS **$4.4–$4.6**, FCF ≥**$5B** [vendor-reported — Zacks].
- Market context: IDC Q2 2026 — HPE ranked **#4 server vendor worldwide**, **$5.87B revenue, 3.5% share, +46.0% YoY** [independent — IDC via StorageReview, https://www.storagereview.com/news/idc-external-storage-tracker-q2-2026-10-3-billion-as-server-market-tops-166-billion].
- ⚠️ **Price gap**: no public per-server pricing found for XD685/XD670/Cray systems (enterprise quotes only).

---

## 2. Giga Computing (GIGABYTE subsidiary)

### 2.1 2026 AI server lineup & launches (dates/specs)

**GB200 NVL4 — XN24-VC0-LA61 (announced Jan 26, 2026)**
- New AI/HPC server on the **NVIDIA GB200 NVL4** platform; debut at **SCA/HPC Asia 2026, Osaka**; heterogeneous CPU+GPU architecture with liquid cooling [official — GIGABYTE press release, https://www.gigabyte.com/press/news/2360].
- **Customer/design win**: XN24 selected for the **RIKEN Center for Computational Science (R-CCS) next-generation HPC-Quantum hybrid platform** — integrated into **FugakuNEXT** (successor to Japan's flagship Fugaku supercomputer) using the **NVIDIA CUDA-Q** platform for hybrid quantum–GPU supercomputing research [official — same release].

**HGX B300 / B200 flagships (OCP EMEA Summit 2026, Barcelona)**
- Two flagship servers on **NVIDIA HGX B300 (liquid-cooled)** and **NVIDIA HGX B200 (air-cooled)**, OCP-standard platforms for AI data centers; plus air- and liquid-cooled compute nodes for rack-level integration [secondary — https://finance.biggo.com/news/dh3B3Z0B-PfaobXfvRh6]. ⚠️ Exact model numbers for the B300 flagships were not stated in the sources consulted.

**G893 series — 8-GPU HGX flagships** (note: launched ~Dec 2024; in-market through 2026)
- **G893-SD1-AAX5** (Intel 5th Gen Xeon) and **G893-ZD1-AAX5** (AMD EPYC): **8× NVIDIA HGX B200**, NVLink with **1800 GB/s GPU-to-GPU bandwidth** [secondary — https://www.pressebox.de/pressemitteilung/giga-computing-technology-co-ltd/GIGABYTE-Unveils-Next-Generation-AI-Flagship-Servers-G893-SD1-AAX5-G893-ZD1-AAX5/boxid/1230117].

**G4L3 / G894 series — HGX B200 thermal-flex portfolio (late 2025)**
- **G4L3-SD1-LAX5** (Intel Xeon) / **G4L3-ZD1-LAX5** (AMD EPYC): 4U direct-liquid-cooled, dedicated CPU & GPU chambers, HGX B200 [secondary — https://www.techpowerup.com/338575/giga-computing-unveils-liquid-and-air-cooled-gigabyte-ai-servers-accelerated-by-nvidia-hgx-b200-platform].
- **G894-AD1-AAX5** (Intel Xeon 6900) / **G894-SD1-AAX5** (Intel Xeon 6700 & 6500): air-cooled HGX B200 [secondary — same source].

**AMD platforms — Advancing AI 2026 (July 2026)**
- New server platforms for **6th Gen AMD EPYC 9006 (SP7 and SP8 sockets)**; full portfolio spanning rack, blade, GPU, edge [secondary — https://www.channelpostmea.com/2026/07/29/giga-computing-adds-support-for-6th-gen-amd-epyc-and-debuts-first-9006-server/ and https://sauditechpost.com/2026/07/30/giga-computing-expands-its-server-lineup-with-6th-gen-amd-epyc/].
- GIGABYTE systems around **AMD Instinct MI350P and MI355X accelerators**, **AMD Pensando Pollara 400 AI NIC**, and **UfiSpace 800G switches** — end-to-end AMD AI infrastructure [secondary — sauditechpost].
- New models: **R165-DG2-CS1** (single-socket 1U, closed-loop liquid cooling, SP7 max 256-core config); **B685-D80-LS1** (6U ten-node blade, DLC, high-density HPC) [secondary — channelpostmea].
- Shipment schedule: **9006 SP7 systems — day-one shipments with AMD in November 2026**; **SP8 systems — March 2027** [secondary — channelpostmea; also https://finance.biggo.com/news/0d245922-df28-40e6-8c8e-0f6badfdbb25].
- Giga Computing GM **Daniel (Hou Chih-jen) Hou** quote: platforms re-engineered from board layout, power delivery, and thermal design for agentic-AI-era demands [vendor-reported via secondary outlets above].

**Vera Rubin** — Giga Computing unveiled an **NVIDIA Vera Rubin rack-scale AI platform at ISC 2026** (June); analysts expect the next-gen Rubin platform launch in **H2 2026** [secondary — https://finance.biggo.com/news/ea828e8b-8abc-436e-9e17-10ecc5fe586d].

### 2.2 Key customers
- **RIKEN R-CCS** (FugakuNEXT HPC-Quantum hybrid platform, Japan) — XN24 GB200 NVL4 [official — https://www.gigabyte.com/press/news/2360]. (The task asked for "key customers" — beyond RIKEN, no 2026 customer names were surfaced in this research; hyperscaler/ODM-style volumes flow through without public disclosure — flagged as a gap.)

### 2.3 Market position (2026)
- **Q1 2026**: GIGABYTE AI server revenue **+110% YoY**, representing **71.5% of total sales**; AI servers now **>80% of total server revenue, approaching 90%** [secondary — https://www.ainvest.com/news/gigabyte-record-revenue-9-margin-ai-server-math-praising-2609/].
- Analyst estimates: GIGABYTE (2376.TW) **Q2 2026 revenue ~NT$140.5B (~$4.4B), +34% QoQ / +37% YoY**, EPS NT$9.08; full-year 2026 revenue **NT$500.5B (~$15.8B), +49% YoY**, EPS NT$31.22; AI server business **67% of 2026 revenue, rising to 72% in 2027** (vs 67% in 2025), AI business growth **+86% (2026) / +37% (2027)** [secondary — biggo finance / analyst note summaries, https://finance.biggo.com/news/ea828e8b-8abc-436e-9e17-10ecc5fe586d].
- Margin context: GIGABYTE gross margin **~9.6% (Q2 2026)** — assembler economics vs NVIDIA's ~75%; Q3 margin recovery expected on motherboard/GPU peak season and GB-series vs HGX mix shift; management stated **H2 revenue and earnings will outpace H1** [secondary — ainvest].
- Market backdrop: IDC Q1 2026 — GPU-accelerated servers **$68.9B (+24.8% YoY), 56.2% of the $122.6B market**; branded OEMs gaining share as ODM Direct share compressed (64.1% → 50.2%) [independent — IDC, https://www.idc.com/resource-center/press-releases/1q26-server-tracker/]; Q2 2026 total $166.3B (+52% YoY) [independent — IDC via StorageReview].
- ⚠️ **Price gap**: no public pricing found for G893/XN24 systems (enterprise quotes only).

---

## 3. ASUS

### 3.1 2026 AI server offerings & launches

**XA NB3I-E12 — 8-GPU NVIDIA HGX B300 (Blackwell Ultra)**
- Shipped **Oct/Nov 2025** (announcement ~326 days before Sept 22, 2026): 8× NVIDIA Blackwell Ultra GPUs, dual **Intel Xeon 6** Scalable CPUs, **8× NVIDIA ConnectX-8 InfiniBand SuperNICs**, 5× PCIe expansion slots, 32 DIMMs, 10× NVMe drives, dual 10Gb LAN [official via PR Newswire/Technode — https://technode.global/prnasia/asus-launches-xa-nb3i-e12-ai-server-built-with-nvidia-hgx-b300/].
- **June 16, 2026 — MLPerf Training v6.0**: #1 rankings in Llama 3.1-8B (Server and Offline modes) and GPT-OSS-120B (Interactive); training scores: 6.584 (Llama 2 7B), 84.85 (DeepSeek 671B), 74.75 (Llama 3.1 8B), 2.342 (DLRM-DCN) [official — https://servers.asus.com/news/ASUS-NVIDIA-HGX-B300-AI-Server-Dominated-MLPerf-Training-v60-Benchmarks]. System "now shipping worldwide" [official — same].

**ASUS AI POD with NVIDIA GB300 NVL72 (Blackwell Ultra rack-scale)**
- 72× Blackwell Ultra GPUs + 36× Grace CPUs per rack, **up to 40 TB high-speed memory per rack**, NVIDIA Quantum-X800 InfiniBand + Spectrum-X Ethernet integration, SXM7 and SOCAMM modules, **100% liquid-cooled**, for trillion-parameter LLM training/inference [secondary — https://markets.hanidaily.com/info/t-2503200966.html (carrying ASUS PR), ~March 2026]. NVIDIA VP Kaustubh Sanghani (GPU products) endorsed ASUS/Blackwell Ultra collaboration [vendor-reported via same].

**ASUS AI POD with NVIDIA Vera Rubin NVL72 (Ai4 2026, Aug 2026)**
- 100% liquid-cooled rack, **densities up to 227 kW per rack**; **XA NR1I-E12L**: 8-GPU **NVIDIA HGX Rubin NVL8** server, hybrid-cooled, as the deskside/entry point to the Rubin architecture [secondary — https://www.businesswire.com/news/home/20260805966459/en/ASUS-Brings-Full-Stack-AI-Infrastructure-From-Data-Center-to-Deskside-to-Ai4-2026].
- Shown at **ASUS AI Tech 2026, Seoul** (Sept 2026): enterprise AI server platforms, HGX training systems, rack-scale AI POD on Vera Rubin NVL72 under the "AI factories" theme [official — https://press.asus.com/blog/asus-ai-tech-2026-seoul-ai-factory-infrastructure/].

**ESC NM2N721-E1 — GB200 NVL72 (customer deployment)**
- NVIDIA GB200 NVL72 architecture (36 Grace CPUs + 72 Blackwell GPUs per rack); deployed in Taiwan's **NCHC "Nano4" AI supercomputer** — Taiwan's first fully liquid-cooled GB200 NVL72 system, in operation [secondary — https://www.techpowerup.com/343414/asus-hardware-powers-taiwans-nchc-ai-supercomputer-ranked-29-on-top500].
- The Nano4 system's NVIDIA **HGX H200** partition (with Direct-to-Chip liquid cooling, 81.55 PFLOPS) ranked **#29 on TOP500** [secondary — same]. Also in the build: ESC8000-E12 servers with **NVIDIA MGX H200** + NVLink Bridge [secondary — same].

**Legacy/in-market lines (carried into 2026)**:
- **ESC8000A-E13P** (MGX, up to 8× NVIDIA H200 NVL dual-slot 600 W, BlueField-3) — shown SC24 [secondary — https://www.techedt.com/asus-unveils-next-generation-infrastructure-solutions-at-sc24-with-advanced-cooling].
- **ESC N8-E11V** (NVIDIA HGX H200) and **ESC N8A-E13** (Blackwell platform) — SC24 2024 showcase [secondary — same]. ⚠️ The task references **ESC N8A-E12**; sources surfaced ESC N8A-E13 (Blackwell) and the earlier ESC N8A-E12 (HGX H100, per press.asus.com CloudFest 2024: 7U dual AMD EPYC 9004 + 8× H100) [official — https://press.asus.com/news/press-releases/asus-mgx-powered-server-cloudfest-2024-showcase/]. Confirming a 2026-spec "ESC N8A-E12" B200/B300 variant was not possible — flagged as a gap.

### 3.2 Customer wins
- **Taiwan NCHC Nano4 AI supercomputer** (national HPC center, academia + industry workloads) — ESC NM2N721-E1 (GB200 NVL72) + HGX H200 systems; #29 TOP500 [secondary — TechPowerUp above].
- **Ubilink** (via ASUS partnership): 45.82 PFLOPS green-energy AI data center [secondary — https://www.tweaktown.com/news/101772/asus-unveils-its-new-ai-pod-complete-rack-of-liquid-cooled-nvidia-gb200-nvl72-servers/index.html].
- ⚠️ No 2026 enterprise/CSP customer deal values found publicly — gap.

### 3.3 Revenue/market context
- ASUS server/AI financials for 2026 were not isolated in this research — gap. (ASUS reports server business within its broader segments; AI POD momentum is the stated growth engine.)

---

## 4. MSI

### 4.1 AI server offerings & 2026 launches

**CG681-S6093 — 6U liquid-cooled AI server (NVIDIA MGX) — unveiled COMPUTEX 2026 (June 2026)**
- Dual **AMD EPYC** processors; up to **8× NVIDIA RTX PRO 6000 Blackwell Server Edition (liquid-cooled)** GPUs; positioned for large-scale AI inference, agentic AI, physical AI, simulation/graphics/video [official — https://www.msi.com/news/detail/MSI-Accelerates-Enterprise-AI-Strategy-with-Cloud-to-Edge-Ecosystem-at-COMPUTEX-2026-148988].
- **NVIDIA ConnectX-8 SuperNICs**, up to 8× 400 Gbps Ethernet; rack-scale: up to **4× CG681-S6093 systems in a 48RU configuration**; networking anchored by **NVIDIA Spectrum-4 SN5600** Ethernet + SN2201 out-of-band switches [official — same].
- COMPUTEX 2026 theme: cloud-to-edge AI continuum — also **NVIDIA DGX Station** and **NVIDIA DGX Spark** deskside AI supercomputers [official — same].

**AMD EPYC 9006 SP7/SP8 platform lineup (2026)**
- MSI launched server platforms on **6th Gen AMD EPYC (SP7 and SP8 sockets)** for AI/cloud/HPC/virtualization modernization; SP7 for high-density/high-performance, SP8 for balanced mainstream [secondary — https://finance.biggo.com/news/0d245922-df28-40e6-8c8e-0f6badfdbb25]. ⚠️ MSI-specific model numbers and ship dates were not detailed in sources consulted — gap (Giga Computing's schedule is above; MSI's was not).

**AI-vRAN (MWC 2026, March 2026)**
- Unified AI-vRAN platform for O-RAN/private 5G/vRAN with NVIDIA AI Aerial; GPU server solutions integrated into network operations [official — https://www.prnewswire.co.uk/news-releases/msi-unveils-scalable-ai-ran-with-nvidia-ai-aerial-solutions-to-accelerate-5g-and-beyond-at-mwc-2026-302695369.html].

**Earlier scalable AI rack (2025, in-market)**
- NVIDIA Enterprise Reference Architecture rack: four-node MGX scalable unit, each node **8× NVIDIA H200 NVL**, **NVIDIA Spectrum-X** networking; expandable to 32 server systems / **256× H200 NVL GPUs** per deployment [secondary — https://itbrief.com.au/story/msi-launches-scalable-ai-server-solutions-with-nvidia-technology].

### 4.2 Gaps for MSI
- No 2026 HGX B200/B300 8-GPU flagship or GB200/GB300 NVL72 rack-scale MSI product was confirmed in this research — possible gap in coverage (MSI's public emphasis in 2026 appears to be MGX inference + EPYC platforms).
- No customer wins or AI-server revenue figures found — gap.
- No public pricing — gap.

---

## 5. xFusion

### 5.1 Company status (post-Huawei spinoff)
- **Spun off from Huawei in 2021** as Huawei restructured under U.S. sanctions; Huawei remains on the U.S. blacklist [secondary — https://www.tekedia.com/huawei-spin-off-xfusion-lines-up-ipo-adviser-as-chinas-ai-stock-boom-draws-in-server-heavyweights/; https://www.sdxcentral.com/news/huawei-server-spin-out-xfusion-looking-to-go-public-in-china-report/].
- Shareholders reportedly include **China Telecom Group Investment** and **China Mobile Capital Holding** (regional reporting, not from a formal prospectus) [secondary — tekedia; https://www.prismnews.com — prismnews notes the CSRC filing did not disclose a definitive owner register].
- **IPO in preparation**: hired **CITIC Securities** as listing tutor (CSRC filing); **no listing venue, timetable, or offering size disclosed** in the filing [secondary — https://www.folio3.ai/ai-pulse/xfusion-hires-investment-bank-potential-ipo-as-china-accelerates-ai-listings/; https://www.prismnews.com article]. Valuation cited at **~$9B in 2023** (Greatwall Strategy Consultants, via Henan government website) [secondary — tekedia/financeneoteric].
- Light Reading: sells general-purpose, AI, and mission-critical servers; reportedly **China's biggest liquid-cooled server supplier**; took strategic investment from a **SASAC-run fund**; Henan government calls it its **"No.1 digital economy project"** [secondary — https://www.lightreading.com/finance/huawei-server-spinoff-xfusion-heads-for-ipo]. The company's global website describes a Singapore company and does **not** identify Chinese state ownership or Huawei-sourced technology [secondary — Light Reading].
- ⚠️ China AI-stock context 2026: Biren Technology (HK debut +76%), Moore Threads (~+400%), MetaX (~+700%); CSI AI Index +67% in 2025; Baidu Kunlunxin filed for HK listing Jan 2, 2026 [secondary — folio3/tekedia]. This context supports the IPO push but says nothing about xFusion fundamentals.

### 5.2 AI server products / 2026 news
- ⚠️ **Thin verifiable product news**: the only product-adjacent 2026 item surfaced was an artificialintelligence-news.com article (~85 days before Sept 22, 2026) describing a "FusionXpark" compliance appliance and "TokenBox" token-generation appliance with NVIDIA DGX OS — this reads as low-quality/synthetic content with unverified product claims **[unverified — do not use without corroboration]**.
- No verifiable 2026 xFusion AI server model launches (e.g., Kunpeng/Ascend-based FusionServer successors) were found in the sources consulted — **flagged as a research gap**; the company's main website is not accessible outside China [secondary — Light Reading].
- Huawei (separate from xFusion) showcased **CloudMatrix 384** as a GB200 NVL72 rival (more memory/bandwidth, ~559 kW power draw — ~4× NVIDIA's) [secondary — https://www.sdxcentral.com/news/huawei-server-spin-out-xfusion-looking-to-go-public-in-china-report/].

### 5.3 Sanctions / export-control implications
- **Huawei** (and 68+ affiliates) has been on the BIS Entity List since **May 16, 2019** (superseding indictment re: Iran sanctions violations); 46 more affiliates added Aug 2019; licensing under presumption of denial; foreign-direct-product rule expanded [independent — BIS Entity List FAQ, https://media.bis.gov/media/documents/dec-3-2020-update-huawei-entity-listing-faqs-posting.pdf].
- ⚠️ **No source found confirming xFusion itself is on the U.S. Entity List** as of Sept 2026 — flagged as unverified/unknown. xFusion was created precisely as a sanctions-mitigation spinoff vehicle, but its post-2021 technology supply relationships (e.g., access to Intel x86 server chips, which sanctions blocked for Huawei [secondary — folio3]) are not documented in the sources found. xFusion reportedly relies on domestic supply chains and state-backed investment instead.
- 2026 policy context: U.S. AI diffusion framework replacement circulated then withdrawn; lawmakers released the **MATCH Act** (Multilateral Alignment of Technology Controls on Hardware) to tighten controls on advanced compute/chipmaking exports to China [secondary — https://fusionlabs.ghost.io/content/files/2026/04/Export-Controls-State-of-Play-April-2026.pdf]. Any xFusion IPO venue/timetable will be shaped by these regimes [secondary — prismnews].

---

## 6. Cross-vendor notes & uncertainties

1. **HPE vs branded-OEM share shift**: IDC Q2 2026 — branded OEMs are capturing a growing share of AI infrastructure (ODM Direct share 60.6% → 53.9% YoY); HPE +46.0% YoY to $5.87B [independent — IDC]. This validates the OEM AI-server wave these vendors ride.
2. **GB300/Vera Rubin timing**: Wccftech (Dec 2025-era report) noted GB300 mass-production concerns and CSP preference for mature HGX 8-GPU systems; GB300 shipments projected +129% YoY in 2026 [secondary — https://wccftech.com/nvidia-blackwell-ultra-ai-servers-to-lead-the-ai-infrastructure-race-moving-into-2026/]. Rubin rack-scale platforms (Giga, ASUS) are staged for **H2 2026** [secondary].
3. **Prices**: no public list pricing was found for any AI server in this wave (all enterprise quote-based) — systematic gap.
4. **Model-number uncertainty**: "ESC N8A-E12" (ASUS) and "Cray XD675" (HPE) appear in the task brief but were not corroborated in 2026 sources; do not use them as confirmed current SKUs.
5. **xFusion product detail**: the weakest area; needs Chinese-language source work (company site, WeChat, CSRC filings) — recommended follow-up by a follow-on research task.
6. **MSI customer/financial detail**: also a gap — MSI's AI-server business scale and 2026 design wins were not found publicly.

**Report written to**: `~/workspace/rag_collect/_draft_etape5_hpe_others.md` (this file)


---

## §3 Data-center networking

# Data-Center Networking for AI (2026)

**Status:** as of 22 September 2026. Report compiled 2026-09-22 (UTC).
**Scope:** switch silicon and systems for AI fabrics (200/400/800 GbE), NVIDIA Spectrum-X / Spectrum-XGS platform, InfiniBand vs Ethernet, UEC / Super Ethernet, optical interconnect (CPO, LPO, 800G/1.6T transceivers, NVIDIA silicon photonics), Arista and Cisco 2026 AI networking business, DCI / 800G adoption figures and 1.6T timeline.
**Provenance key:** every bullet carries a tag — `[official]` = vendor press release / investor-relations page; `[vendor-reported]` = company claim via earnings call or company blog (not independently audited); `[independent]` = independent press (ServeTheHome, Data Center Dynamics, SemiEngineering, EE Times, etc.) or analyst firm data reproduced faithfully; `[secondary]` = trade/financial press or analyst summaries; `[unverified]` = plausible but not independently confirmed by a named source. **Conflicts and gaps are flagged explicitly; identifiers are only quoted when found in a source.**

---

## 0. Executive summary

- 800G is the mainstream AI-fabric generation in 2026; 1.6T is shipping in early volume and is expected to ramp in H2 2026. [independent] [secondary]
- Dell'Oro Group (Q1 2026 quarterly snapshot, published June 2, 2026): Ethernet switch sales in AI backend networks "more than doubled" YoY and were "about two-thirds of data center switch sales in AI clusters"; InfiniBand sales "more than tripled" in the quarter, supported by the 800 Gbps switch ramp with NVIDIA's Blackwell Ultra platform; Dell'Oro cautioned the IB rebound may partly be brownfield upgrades, not greenfield expansion. Same report: Celestica #1 in Ethernet AI backend networks, followed closely by NVIDIA, Arista third, Cisco fourth (largest share gain in the quarter). [independent] (analyst data cited via gpusmith summary of delloro.com)
- NVIDIA is now the largest data center Ethernet switching vendor per Dell'Oro figures cited by secondary press (~$2.1B quarterly switch revenue, 21.5% share in 2026, up from <4% two years ago). [secondary]
- Broadcom's Tomahawk 6 (102.4 Tbps, 512×200G SerDes, 3nm) reached production volume March 12, 2026. [official]
- NVIDIA announced Spectrum-6 (102.4 Tbps Ethernet switch, part of the Rubin stack) in July 2026 and Spectrum-XGS Ethernet (scale-across, software/firmware upgrade, ~2× NCCL throughput claim) — both [official].
- UEC Specification 1.0 released June 11, 2025; 1.0.1 in September 2025; current 1.0.3 as of July 16, 2026 (per community wiki; confirm against ultraethernet.org). Broadcom Thor Ultra NIC (Oct 2025) was the first NIC built to the spec; Keysight–Broadcom demoed UEC LLR/CBFC at 800GE at OFC 2026. [independent] [unverified]
- CPO entered the market in 2026 with early products from NVIDIA and Broadcom, but large-scale high-volume CPO is widely expected only ~2028–2030; LPO is shipping in volume in 2026 (millions of units expected). [independent]
- TrendForce: 800G+ optical transceiver shipments ~24M units (2025) → ~63M units (2026), 2.6×; 1.6T shipments ~2.5M (2025) → 20M+ by end-2026. EML laser capacity is a bottleneck; NVIDIA has pre-allocated large EML capacity. [secondary]
- Arista Q2 2026: record $3.036B revenue (+37.7% YoY), raised FY2026 AI-fabrics revenue target to at least $3.5B (at least $3.6B per Zacks), overall FY2026 target ~$11.5B (Aug 2026); one secondary outlet (remio.ai, Sept 19, 2026) claims a raise to ~$12.6B — **conflicting, unverified**. Etherlink customer base >100. [official] [secondary]
- Cisco FY2026: $9.3B AI infrastructure orders from hyperscalers (~4.5× FY2025), ~$4B recognized revenue, FY2027 AI revenue guide $7.5B; Silicon One G300 (102.4 Tbps) introduced; Cisco also sells NVIDIA Spectrum-X-powered N9100 switches. [vendor-reported] [secondary]

---

## 1. 200/400/800 GbE switch adoption in AI clusters

### 1.1 Adoption picture (Dell'Oro Q1 2026)

- Dell'Oro Group's most recent quarterly snapshot (published June 2, 2026) on AI backend networks: "Ethernet switch sales in AI backend networks more than doubled and accounted for about two-thirds of data center switch sales in AI clusters during the first quarter 2026," while "InfiniBand sales more than tripled during the quarter, supported by the ramp of 800 Gbps switches shipping with NVIDIA's Blackwell Ultra platform." [independent] (cited via gpusmith summary of delloro.com)
- Dell'Oro's analysts cautioned the InfiniBand rebound "may partly reflect upgrades of the installed base in brownfield deployments rather than greenfield expansion." [independent]
- Vendor share in Ethernet AI backend networks (Q1 2026): "Celestica regained the leading position, followed closely by NVIDIA," with "Arista ranked third" and "Cisco recorded the largest share gain during the quarter and ranked fourth." [independent]
- Same source: "800 Gbps switches accounted for the vast majority of the Ethernet switch shipments and revenues in AI backend networks during the quarter," with 1600 Gbps switches "only beginning to sample" and expected to "ramp in the second half of 2026." [independent]
- Current-generation switch silicon across both IB (Quantum-X800) and Ethernet (Spectrum-X, Tomahawk 5/6, Silicon One G200) sits at roughly the same 800 Gb/s per-port tier. [secondary] (temperature2.com)

### 1.2 Broadcom Tomahawk 5 / Tomahawk 6

- **Tomahawk 6 family (TH6):** production volume shipments announced **March 12, 2026** [official] (GlobeNewswire via PR). Key specs: 102.4 Tbps single-chip switching capacity (2× Tomahawk 5); 512 × 200G SerDes (or 1024 × 100G); 3nm (TSMC, per secondary reporting); support for 100G and 200G SerDes; CPO (co-packaged optics) option; AI routing features incl. Cognitive Routing 2.0 and Global Load Balancing; targets >1M-XPU clusters; 128K-XPU fabrics in two tiers; single-chip 512-XPU connectivity. [official]
- Sameh Boujelbene (VP, Dell'Oro Group), quoted in Broadcom's release: TH6 addresses "large-scale, low-latency fabrics with fewer switch tiers and reduced optical complexity." [vendor-reported]
- Arista and Cisco have integrated TH6 into latest chassis per secondary coverage (stocktitan/financialcontent). [secondary]
- **Tomahawk 5 (TH5):** 51.2 Tbps, 100G/200G SerDes, the prior generation; base of Arista's 7060X6-class and other 800G platforms. [secondary]
- **Thor Ultra NIC:** Broadcom announced October 14, 2025 as the first NIC built to the UEC 1.0 specification. [secondary] (temperature2.com citing Broadcom)

### 1.3 Arista (7060X6, 7800R4, 7700R4, 7060XE7, Etherlink)

- **Etherlink AI portfolio:** Arista's AI-optimized Ethernet portfolio; customer base expanded to **over 100 customers** (Q2 2026), up from just 4–5 in 2024. [vendor-reported] (cryptobriefing summarizing Arista Q2 2026 earnings)
- During Q2 2026 Arista introduced its **7506c/7 Series AI fabric portfolio** and **1.6 Tbps platforms with liquid-cooled options** (also reported as the **7060XE7 Series 1.6 Tbps platforms**) for the largest AI training/inference clusters. [vendor-reported] [secondary]
- Arista's official Q2 2026 release notes: "Introduced 1.6 Tbps AI fabric platforms, including liquid-cooled options optimized for scale-up, scale-out, and scale-across networks." [official]
- The 7060X6 (Tomahawk 5-based 800G) and 7800R4/7700R4 (modular 800G systems) lines are Arista's shipping 800G portfolio for AI backends; Meta's RoCE-over-Ethernet 24,576-GPU H100 cluster for Llama 3 used Arista 7800 systems side-by-side with a Quantum-2 InfiniBand cluster, both reaching 90%+ network utilization. [secondary]
- **Uncertainty flag:** I did not independently confirm per-SKU shipment volumes for the 7060X6/7800R4/7700R4; the task's "shipments" granularity is not disclosed at SKU level in the sources found. Do not fabricate SKU shipment numbers.

### 1.4 Cisco Nexus 9000 800G / Silicon One

- Cisco introduced **Silicon One G300** — its own 102.4 Tbps networking silicon — debuting in **liquid-cooled N9000 and 8000 series switches**, aimed at competing with Broadcom and NVIDIA switching chips. [secondary] (ainvest)
- Simultaneously, Cisco ships **N9100 series switches explicitly powered by NVIDIA Spectrum-X Ethernet silicon**, running Cisco NX-OS on NVIDIA hardware. [secondary] (ainvest)
- Cisco FY2026 hyperscaler AI design wins: three new wins in Q4 alone — one **Silicon One P200 scale-across** deployment, one **G200 scale-out** project, and one optical line-system deployment; management flagged line-of-sight to more wins over the next six months across G300, G200, P200 and A100 platforms. [vendor-reported] (infotechlead)
- TrendForce platform table (2025 vintage, cited 2026): Cisco Silicon One G200 series at 51.2 Tbps (2023) alongside Marvell Teralynx 10; CPO prototypes. [secondary]

### 1.5 NVIDIA Spectrum-X switches (SN5000-class / SN5600 / Spectrum-X800)

- **Spectrum-X800 Ethernet switch** (SN5600-class): 800 Gb/s per port, 51.2 Tb/s switching capacity (per temperature2.com / TrendForce table). Based on the **Spectrum-4 switch ASIC**; SN5600 = 64-port 800GbE. [secondary]
- **Spectrum-6** (announced July 2026): 102.4 Tbps Ethernet switch system, 2× previous-generation capacity, integrated into the Rubin-architecture stack (Vera CPU, Rubin GPU, NVLink 6, ConnectX-9 SuperNICs, BlueField-4 DPUs, Spectrum-6). [vendor-reported] (cxotoday citing NVIDIA)
- TrendForce roadmap table: Spectrum-X800 (800G/port, 51.2 Tbps) → Spectrum-X1600 (1.6T/port, 102.4 Tbps, paired with Rubin) → Spectrum-X3200 (3.2T/port, 204.8 Tbps). ConnectX-8 (800G, PCIe 6.0) → ConnectX-9 (1.6T, PCIe 7.0) → ConnectX-10 (3.2T, PCIe 8.0). [secondary]

### 1.6 Celestica's position

- Celestica regained #1 in Ethernet AI backend networks in Q1 2026 per Dell'Oro (above). Celestica is NVIDIA's primary Ethernet-switch manufacturing partner for Spectrum-X systems; its ranking reflects white-box/OEM volume into AI fabrics. [independent] [secondary]

---

## 2. NVIDIA Spectrum-X / Spectrum-XGS Ethernet Networking Platform

### 2.1 Platform composition (per NVIDIA)

- Spectrum-X = Spectrum Ethernet switches + Spectrum-X/ConnectX SuperNICs + BlueField DPUs + LinkX cabling and transceivers (+ Spectrum-XGS for multi-DC). [official]
- Performance claim: 1.6× greater bandwidth density than off-the-shelf Ethernet for multi-tenant hyperscale AI factories, "including the world's largest AI supercomputer." [official] (investor.nvidia.com Spectrum-XGS release)
- NVIDIA reports Spectrum-XGS "nearly doubles the performance of the NVIDIA Collective Communications Library (NCCL)," accelerating multi-GPU/multi-node communication across geographically distributed clusters. [official]
- Delivery mechanism for XGS: primarily **software and firmware updates to existing Spectrum-X switches and ConnectX SuperNICs**, not new silicon. [secondary] (techpowerup)

### 2.2 Spectrum-XGS (scale-across), available now

- Announced by NVIDIA (investor.nvidia.com; circa September 2025 based on the release cadence and coverage; the "available now" release was covered at Hot Chips). Adds a **"scale-across"** axis beyond scale-up/scale-out, linking data centers across cities, nations and continents into "giga-scale AI super-factories." [official]
- Technologies: auto-adjusted distance congestion control, precision latency management, end-to-end telemetry; algorithms dynamically adapt the network to inter-facility distance. [official]
- **CoreWeave** named as one of the first adopters, connecting its data centers into a single supercomputer; CTO quote from Peter Salanki in the release. [official]
- Spectrum-XGS is fully integrated into the Spectrum-X platform. [official]
- Arista has publicly identified **scale-across** as a separate networking opportunity (its 1.6T platforms target scale-up/scale-out/scale-across), mirroring NVIDIA's framing. [vendor-reported] [secondary]
- **Uncertainty flag:** the exact announcement date of the Spectrum-XGS release could not be verified from the investor.nvidia.com page text fetched; the techpowerup URL (340218) and Hot Chips mention place it in the September 2025 timeframe. Treat the date as approximate.

### 2.3 Spectrum-6 (Rubin era, July 2026)

- Announced ~July 23, 2026 (cxotoday). 102.4 Tbps Ethernet switch system, 2× previous-gen capacity; designed to connect hundreds of thousands of GPUs with improved power efficiency; part of the Rubin AI infrastructure stack alongside Vera CPU, Rubin GPU, NVLink 6 switches, ConnectX-9 SuperNICs, BlueField-4 DPUs. [vendor-reported] (cxotoday citing NVIDIA)
- NVIDIA quarterly networking revenue context (recent quarter as of July 2026): **$11B**, "massive over 250% growth over the previous year"; CEO Jensen Huang declared "We're now the largest networking company in the world." [vendor-reported] (cxotoday citing NVIDIA)
- Per Dell'Oro figures cited by secondary press (2026): NVIDIA ~$2.1B quarterly data-center Ethernet **switch** revenue, 21.5% share — the largest Ethernet-switch vendor, up from <4% two years ago. (Note: this is switch revenue only, not total NVIDIA networking revenue.) [secondary]

### 2.4 Spectrum-X adoption and performance claims

- **xAI Colossus (flagship win):** 100,000 Hopper GPUs (Memphis) built in 122 days using Spectrum-X Ethernet (SN5600 64-port 800GbE switches, BlueField-3 SuperNICs, 400GbE per GPU) for the RDMA network — instead of InfiniBand. NVIDIA claims: 95% data throughput maintained, zero application latency degradation/packet loss from flow collisions across all three fabric tiers; standard Ethernet would deliver only 60% throughput with thousands of flow collisions. xAI doubling to 200,000 Hopper GPUs; Colossus 2 adds ~30,000 GB200s. [official] (nvidianews.nvidia.com)
- Spectrum-X closes 80–90% of the InfiniBand gap on NCCL all-reduce — within ~5% at 8 nodes, widening at 64+ nodes (cited by rdp.in from Spheron, 2026). [secondary]
- Inference/multi-tenant positioning: Spectrum-X Ethernet ~5–10% behind IB on effective all-reduce at 8 nodes, ~1.7 µs vs ~0.9 µs latency; recommended for inference, multi-tenant and mixed clouds. [secondary]
- DriveNets claimed (Aug/Sep 2026) its AI Fabric Ethernet solution achieved 6% better NCCL performance than NVIDIA InfiniBand (Quantum-X) on average and 15% better than Spectrum-X on a WhiteFiber Iceland deployment — vendor claim using SemiAnalysis test data; treat as [vendor-reported] and un-audited.

### 2.5 Spectrum-4 ASIC context

- Spectrum-X800 SN5600 switches are based on the Spectrum-4 switch ASIC (800G ports, 51.2T). [secondary] (tomshardware citing NVIDIA)

---

## 3. InfiniBand vs Ethernet in 2026 AI fabrics

### 3.1 2026 state of play

- Both NVIDIA Quantum-X800 (InfiniBand) and Spectrum-X800 (Ethernet) now ship at 800 Gb/s per port; Quantum-X800 at 115.2 Tb/s total switching capacity vs Spectrum-X800 51.2 Tb/s per the ascentoptics comparison (note: different switch configurations; treat as [secondary], vendor/product-page sourced). [secondary]
- 8-node reference numbers (secondary, 2026): IB NDR 400G (Quantum-2) ~350 GB/s effective all-reduce at sub-1 µs latency with SHARP in-switch reductions; well-tuned 400GbE RoCEv2 ~270–290 GB/s at ~2.4 µs; Spectrum-X within 5–10% of IB. [secondary] (rdp.in, temperature2.com)
- Decision guidance in 2026 (secondary consensus): IB for large training where all-reduce latency compounds every step; Ethernet (Spectrum-X/RoCE) for inference, multi-tenant clouds, and open-ecosystem deployments. [secondary]
- **Meta example:** two 24,576-H100 clusters run side by side — one RoCE-over-Ethernet (Arista 7800), one NVIDIA Quantum-2 InfiniBand — both reaching 90%+ network utilization training Llama 3. [secondary] (temperature2.com)
- Dell'Oro Q1 2026: Ethernet ~2/3 of AI-backend DC switch sales; IB sales tripled on the Blackwell Ultra 800G ramp (brownfield-upgrade caveat noted). [independent]

### 3.2 NVIDIA InfiniBand updates: Quantum-X800 / Quantum-3 / Quantum-X

- **Quantum-X800 (XDR InfiniBand, 800G/port):** shipping with NVIDIA's Blackwell Ultra platform ramp (Dell'Oro Q1 2026: IB sales "more than tripled ... supported by the ramp of 800 Gbps switches shipping with NVIDIA's Blackwell Ultra platform"). [independent]
- Quantum-X800 features SHARP v4 in-network computing for collective operations (9× improvement claim in the ascentoptics comparison table — [secondary], treat improvement factor as vendor-reported via that source). [secondary]
- NVIDIA newsroom framed the IB vs Ethernet choice for xAI: Colossus chose Spectrum-X Ethernet over InfiniBand — a landmark design decision for the world's largest AI supercomputer at the time. [official]
- **Quantum-X silicon photonics switches:** NVIDIA's announced Quantum-X and Spectrum-X silicon photonics networking switches "enable AI factories to connect millions of GPUs across sites while reducing energy consumption and operational costs" — referenced in the Spectrum-XGS release. [official]
- TrendForce roadmap: Quantum-2 (400G/port, 51.2 T) → Quantum-X800 (800G, 115.2 T) → Quantum-X1600 (1.6T, 230.4 T, paired with Rubin) → Quantum-X3200 (3.2T, paired with Feynman). [secondary]
- **Uncertainty flags:** (a) No Quantum-3 shipping announcement was found in the sources retrieved; the platform table lists Quantum-2 → Quantum-X800 → Quantum-X1600, so "Quantum-3" may be a skipped/renamed generation — do not assert its existence. (b) Quantum-X800 CPO/silicon-photonics variant shipping dates were described as "early 2026" roadmap (per the github silicon-photonics research note) but no dated production shipment was confirmed. (c) The `<100 ns` latency figure in the ascentoptics table vs "sub-1 µs" elsewhere is inconsistent — do not quote either as settled fact.

### 3.3 Broadcom vs NVIDIA "scale-out tech war"

- TrendForce (2025, still cited 2026): frames the AI fabric contest as Broadcom (Tomahawk/Thor/UEC) vs NVIDIA (Spectrum-X/Quantum-X) camps; Marvell Teralynx 10 and Cisco Silicon One G200 (both 51.2T, 2023) also compete; Cisco has CPO prototypes. [secondary]

---

## 4. UEC (Ultra Ethernet Consortium) / Super Ethernet

### 4.1 Specification status

- **UEC Specification 1.0:** released **June 11, 2025**; 562-page document defining the full stack (physical layer → transport → application API), including the new **Ultra Ethernet Transport (UET)** protocol with unordered packet delivery, receiver-driven congestion control, native multipath/packet spraying, and operation without mandatory lossless-fabric (PFC) configuration. [independent] (LLM-Systems-Wiki, fetched 2026-08-25, primary sources = UEC press releases + spec)
- Maintenance updates: **1.0.1 (September 2025)** [independent] (networkworld), **1.0.2 and 1.0.3** release notes; **1.0.3 (July 16, 2026) is current** [unverified — sourced from a community wiki; verify against ultraethernet.org]. UEC 2.0 development expected/ongoing (expected 2026 per a June-2025-dated secondary blog; no confirmed 2.0 release found as of Sept 2026). [unverified]
- IEEE 802.3dj (200G/400G/800G/1.6T at 200G/lane) on track for completion late 2026; a 400G/lane follow-on project is being prepared. [independent] (networkworld, quoting Ethernet Alliance chair Peter Jones)
- **Why UEC exists (per consortium framing):** RoCEv2's PFC+DCQCN lossless requirements cause head-of-line blocking, congestion spreading, single-hash-path flows (no packet spraying), and ~30% performance loss at scale vs InfiniBand. UET is a clean-slate transport. [secondary] (Medium/Teradata Labs piece — note: vendor-adjacent authorship, treat the 30% figure as consortium-aligned advocacy, not measured benchmark)

### 4.2 Member momentum

- Founded **July 19, 2023** (Linux Foundation Joint Development Foundation project) by AMD, Arista, Broadcom, Cisco, Eviden/Atos, HPE, Intel, Meta, Microsoft. [independent]
- Consortium has grown to **100+ member companies and ~1,500 participants** (2026). [secondary] (Medium/Teradata Labs)
- Compliance/certification programs "already underway" per UEC launch materials; Keysight's OFC 2026 demo shows the test-equipment ecosystem aligning. [official] (Keysight bizwire via financialcontent)

### 4.3 Products shipping with UEC in 2026

- **Broadcom Thor Ultra NIC** (announced Oct 14, 2025): first NIC built to the UEC 1.0 spec. [secondary]
- **Broadcom Tomahawk Ultra Ethernet switch** (UEC-compliant, LLR + CBFC features): used in the Keysight interop demo at **OFC 2026** — "industry's first public interoperability demonstration of Ultra Ethernet Consortium (UEC) specification with Link Layer Retry (LLR) and Credit-Based Flow Control (CBFC) at 800GE line rate," with Keysight's Interconnect and Network Performance Tester. [official] (Keysight press release)
- LLR (local error recovery, lower tail latency) and CBFC (link-layer flow control) identified as the UEC link-layer capabilities hyperscalers now require for large AI clusters. [official] (Keysight)
- Arista announced support for UEC across its **Etherlink** portfolio. [secondary] (Medium/Teradata Labs — [unverified] detail; no dated Arista press release located)
- **Reality check:** the UEC endgame pieces framing (Medium, June 2026) also notes a Broadcom "UE-compatible Tomahawk switch with 250 nanosecond latency" — [unverified], single-source advocacy; do not quote as fact.
- **Uncertainty flag:** no hyperscaler production deployment of UEC transport (UET) was confirmed in any source; 2026 activity is interop demos, NIC/switch silicon, and test equipment. UEC adoption in production AI fabrics is best described as "pre-commercial."

---

## 5. Optical interconnect

### 5.1 Co-packaged optics (CPO) status 2026

- CPO **entered the market in 2026** through early products from **NVIDIA and Broadcom** (early AI-networking deployments). [secondary] (eetimes, citing analyst Vladimir Kozlov)
- Realistic assessment: "**it's going to take another two years before CPO is really shipping in volumes**" — large-scale CPO deployment realistically **2028–2030** (Yole Group forecast cited in the github research note); current deployments are pilot/proprietary, focused on scale-up AI networks. [independent]
- June 9, 2026: SemiAnalysis report flagged CPO packaging yield risk (~19.4% system yield in a 0.95^32 scenario), triggering optical-stock selloffs (AAOI −17%, COHR −11%, LITE −8%); NVIDIA CTO Gilad Shainer publicly disputed the thesis, stating CPO is already shipping and ramping H2 2026. [secondary] [unverified detail]
- Broadcom Tomahawk 6 supports CPO options; Cisco has CPO prototypes; TSMC COUPE is the foundry vehicle. [official] [secondary]
- Intel's OCI chiplet claim of 5 pJ/bit vs 15 pJ/bit legacy systems is a real physics datapoint (vendor claim). Indium/InP supply concentration (China) is a physical chokepoint; external light sources for CPO are an emerging ~$1B+/yr sub-market. [secondary] (github research note — [unverified] figures)

### 5.2 Linear pluggable optics (LPO)

- LPO removes the DSP from the module; 200G LPO targets: ~10 W/module vs 23–25 W retimed DSP modules and ~16 W linear-receive (RTLR) modules; a 512-port Tomahawk-6 switch saves ~500 W at module level (nearly 1 kW with cooling) moving from retimed to LPO. 200G LPO target reach 500 m (3 dB optical budget); up to ~2 km in low-dispersion DR with sufficient budget. [independent] (itbrief.co.nz citing Semtech)
- Deployment status: LPO "already in use by four or five companies, including Oracle"; Google has started deploying **1.6T linear receive optics** and is preparing volume rollouts in 2026; millions of LPO modules expected to ship in 2026, tripling from 2025 to 2030. [independent] (eetimes, citing analyst Vladimir Kozlov)
- LPO market (Dataintelo research report, 2026): $2.8B in 2025 → $14.7B by 2034, ~20.2% CAGR 2026–2034. (Market-research firm estimate — treat methodology as [secondary], unverifiable.) [secondary]
- Trade-off: lower module power with narrower operating margins; the engineering burden shifts to the host ASIC and board design; signal-integrity at 224G SerDes is the key scaling challenge; standards/interop for LPO are still evolving. [secondary] (eedesignit)
- Players exploring LPO: Broadcom, Marvell, Microsoft, Meta, Google. [secondary]
- Kozlov (eetimes): "The adoption of LPO and CPO in scale-up networks is expected to accelerate in 2026–2027 and reach high volumes by 2028."

### 5.3 800G / 1.6T optical transceiver market (2026)

- TrendForce: **800G+ shipments ~24M units (2025) → ~63M units (2026)**, 2.6× growth; 1.6T shipments **~2.5M (2025) → 20M+ by end-2026** (per marketminute coverage of OFC 2026 analyst confirmation; 1.6T "scaling faster than any previous generation"). [secondary]
- LightCounting-derived TAM (cited in a July 2026 investor research note): total transceivers $23.8B (2025); datacenter modules ~$22.8B (2026), of which **800G+1.6T ~$14.6B**; 800G+ units ~24M (2025) → ~63M (2026). Note: scope disagreements are wide (MarketsandMarkets pegs DC transceivers at only $9.2B for 2025) — treat single points cautiously. [secondary] (github/roachx92 citing LightCounting)
- GM Insights (market-research firm, 2026): optical transceiver market $13.4B (2025) → $15.4B (2026) → $48.1B (2035), 13.5% CAGR; Coherent led with >22.2% share in 2025; top-5 (Coherent, Cisco, Broadcom, Lumentum, Accelink) held 72.2%. (Estimate — [secondary].) [secondary]
- Vendor landscape: **Innolight (~$5.6B datacenter modules)** and Eoptolink together hold **~60% of NVIDIA's 800G volume**; Coherent and Lumentum are the Western integrated laser suppliers (Lumentum the only volume 200G-EML shipper at ~50–60% share, +40% EML capacity in 2025 and again 2026); Coherent expanding 6-inch InP across 4 sites (~4× die/wafer at ~½ cost, capacity doubling 2026); Cisco (Acacia) took >$1B of optics orders in a single quarter (Q4 FY2026). [secondary] [vendor-reported] (mix: github research note citing J.P. Morgan/LightCounting; Cisco via ainvest)
- **EML bottleneck:** TrendForce reports an upstream laser bottleneck; NVIDIA pre-allocated large EML capacity (investor note claims "$4B lock-up of Lumentum+Coherent EML capacity" with multiyear purchase commitments — [unverified]); NVIDIA's March 2026 $2B direct investment in Lumentum to guarantee photonics capacity (marketminute). Extended EML lead times beyond 2027 reported. [secondary]
- Adopters: "1.6T is shipping now," with **NVIDIA and Google already integrating**; Meta and Oracle slated to follow (per Kozlov via eetimes). [independent]

### 5.4 NVIDIA silicon photonics

- NVIDIA's roadmap includes **Spectrum-X Ethernet Photonics (H2 2026)** and Quantum-X InfiniBand (early 2026) — i.e., CPO/silicon-photonics switch variants (per the github silicon-photonics research note). [unverified] — no dated NVIDIA product announcement of these variants was located in vendor sources; treat H2 2026 as roadmap guidance, not confirmed shipping.
- NVIDIA announced **Quantum-X and Spectrum-X silicon photonics networking switches** (referenced in the Spectrum-XGS release as enabling "AI factories to connect millions of GPUs across sites while reducing energy consumption and operational costs"). [official]
- NVIDIA CTO Gilad Shainer: CPO "already shipping and ramping H2 2026" (response to June 2026 SemiAnalysis yield-risk report). [vendor-reported]
- NVIDIA's slower-than-anticipated silicon-photonics/CPO progress is one reason for continued dependence on pluggable modules and the EML pre-allocation strategy (TrendForce). [secondary]

### 5.5 Data center interconnect (DCI) notes

- 800G digital coherent optics (DCO): $963M market (2025) → $1,713M (2032), 8.7% CAGR (MarketPublishers, Jan 2026 — [secondary] estimate); key players II-VI/Coherent, Lumentum, Innolight, Hisense, Accelink. [secondary]
- Scale-across DCI is emerging as its own category: Cisco's Silicon One **P200** is explicitly positioned for scale-across deployments (three hyperscaler wins in Q4 FY2026 incl. a P200 scale-across win); NVIDIA Spectrum-XGS is the Ethernet answer; Arista's 1.6T platforms target scale-across. [vendor-reported] [official]
- Corning announced a multi-billion-dollar "rack-scale" fiber infrastructure deal with Meta for gigascale AI data centers (March 2026). [secondary] (marketminute)

---

## 6. Arista: 2026 AI networking revenue, wins, earnings

### 6.1 Q2 2026 results (reported August 4, 2026)

- Revenue **$3.036B** (+12.1% QoQ, **+37.7% YoY**) — first $3B quarter. GAAP operating margin 45.4%, non-GAAP 49.9%. Non-GAAP diluted EPS $1.02 (+39.7% YoY); GAAP EPS $0.95. [official] (Arista Q2 2026 release; also hosted at s21.q4cdn.com)
- CEO Jayshree Ullal: "As we deliver our first $3 billion quarter in Q2 2026, it is clear that our Arista 2.0 platform strategy is compelling. Customers see networking as the central nervous system for infrastructure from the client to campus to data and AI centers." [official]
- CFO Chantelle Breithaupt: "Our second quarter 2026 reflects strong, broad-based growth, with revenue up 37.7% and EPS up 39.7%." [official]
- Recognized on the 2026 Fortune 500 list; named a Leader in the 2026 Gartner Magic Quadrant for Enterprise Wired and Wireless LAN. [official]
- Q1 2026 (reported ~May 2026): $2.71B revenue (+35.1% YoY), diluted EPS $0.87 (+31.8%), raised 2026 guidance to $11.5B with $3.5B from AI. [secondary] (fxempire)

### 6.2 Guidance and AI-fabrics revenue

- At Q2 2026: management raised the **AI-fabrics revenue target to at least $3.5B for FY2026** (more than double the prior year); Zacks (Sept 16, 2026) quotes "at least $3.6B" supported by scale-up, scale-out and scale-across deployments. [vendor-reported] [secondary]
- Overall FY2026 revenue outlook raised to **approximately $11.5B** (≈27.7% growth) per August-2026 coverage. [secondary] (cryptobriefing, fxempire)
- **Conflict flag:** remio.ai (published ~Sept 19, 2026) states Arista "raised its 2026 revenue target to $12.6 billion" after Q2, with Q3 guidance ~$3.3B — this conflicts with the $11.5B figure from August coverage. No corroborating source found. Possible misread (e.g., annualizing Q3 guide) or a later raise not yet covered elsewhere. **Treat $12.6B as [unverified]/conflicting.**
- Etherlink AI customer base: **over 100 customers** (vs 4–5 in 2024); Microsoft and Meta identified as key customers. [vendor-reported] [secondary]
- Consensus estimates (TIKR, Aug 2026): $12.67B (2026), $16.16B (2027), ~$26.54B by 2030; 2021–2025 revenue grew $2.95B → $9.0B. [secondary]
- Zacks (Sept 16, 2026): stock +47% YTD 2026; 52-week range $114.52–$214.89; AI revenue at least $3.6B in 2026; projected FY26 revenue growth ~39%. [secondary]

### 6.3 Product launches (2026)

- 1.6 Tbps AI fabric platforms (7060XE7 series), incl. liquid-cooled options; 7506c/7 Series AI fabric portfolio; 7th-gen 7280R series for AI workloads. [vendor-reported] [secondary]
- Arista positioned across scale-up, scale-out, and scale-across networks. [official]

### 6.4 xAI/Colossus relationship

- xAI Colossus chose **NVIDIA Spectrum-X Ethernet**, not Arista — a design win for NVIDIA, not Arista. Arista's hyperscaler AI exposure is via Microsoft, Meta, and other cloud builders (RoCE/Ethernet fabrics, e.g., Meta's Arista 7800-based 24,576-GPU cluster). [official] [secondary]
- Competitive pressure: NVIDIA's integrated stack (GPUs + Spectrum-X + SuperNICs + optics + software) is the structural challenge to Arista's open-Ethernet position, per secondary analysis. [secondary]

---

## 7. Cisco: AI networking (Silicon One, Nexus), 2026 wins and revenue

### 7.1 FY2026 results (year ended July 2026; reported ~Aug 2026)

- Q4 FY2026 revenue **$17.25B** (+18% YoY, record); full-year revenue **$63.33B** (+12% vs $56.65B FY2025); product revenue $48.30B, services ~$15.03B. [secondary] (infotechlead)
- **AI infrastructure orders from hyperscalers: $9.3B in FY2026** (~4.5× the ~$2B of FY2025), incl. **$4B in Q4 alone** (record quarter for AI orders). [vendor-reported]
- Order mix: ~60% Silicon One-based systems, ~40% optics. [vendor-reported]
- **Recognized AI infrastructure revenue from hyperscalers: ~$4B in FY2026** (vs $1B FY2025); AI infra was ~6% of total revenue (vs <2% in FY2025). [vendor-reported]
- **FY2027 guide: total revenue $72.2B–$73.4B (~15% growth at midpoint); AI infrastructure revenue from hyperscalers $7.5B** (CFO said the rest of the business would still grow ~10%); Cisco retired the order-booking target framework in favor of a formal revenue target. [vendor-reported] (trefis, ainvest)
- Price increases added ~5 points to FY2026 Q4 revenue growth (CFO); 4–5 points planned for FY2027 (front-half weighted); memory costs and hardware mix pressuring gross margin. [vendor-reported] (trefis)
- Non-hyperscaler AI: >$400M of AI infra orders from neocloud, sovereign and enterprise customers in Q4; full-year >$1B from those customers. [vendor-reported]

### 7.2 Design wins (Q4 FY2026)

- Three new hyperscaler AI design wins: one **Silicon One P200 scale-across** deployment, one **G200 scale-out** project, one **optical line-system** deployment; four leading hyperscale customers; line of sight to more wins over six months across G300, G200, P200, A100 and optics. [vendor-reported]
- Acacia optics business: >$1B of orders in a single quarter (Q4). [vendor-reported] (ainvest)
- Products: Silicon One **G300** (102.4 Tbps) in liquid-cooled N9000 and 8000 series; **N9100** series powered by NVIDIA Spectrum-X silicon with NX-OS; Series 8000 routers; Acacia coherent pluggables. [secondary] [vendor-reported]

### 7.3 Strategic position / caveats

- Cisco lost the DC switching crown to Arista; NVIDIA then overtook both in DC Ethernet switching share (Dell'Oro figures cited by secondary press). [secondary]
- Commentary (ainvest, editorial): Cisco's strategy mixes own-silicon (G300) with reselling NVIDIA's Spectrum-X platform — described as defensive. [secondary, opinion]
- Margin caution: the AI orders are in a competitive, buyer-leverage-heavy segment; Cisco's operating margin (TTM 25.4%) vs Dell 9.6% noted as the offset. [secondary] (trefis)

---

## 8. Data center interconnect: 800G adoption, 1.6T timeline

- **800G is the mainstream AI-fabric generation:** "the vast majority of Ethernet switch shipments and revenues in AI backend networks" in Q1 2026 were 800G (Dell'Oro). [independent]
- **Optical volume:** 800G+ shipments 24M units (2025) → ~63M (2026) (TrendForce). [secondary]
- **1.6T timeline:** 1600G switches "only beginning to sample," expected to "ramp in the second half of 2026" (Dell'Oro, June 2026). 1.6T optical shipments 2.5M (2025) → 20M+ by end-2026 (OFC-2026 analyst confirmation); "1.6T is shipping now" with NVIDIA and Google integrating, Meta and Oracle slated next (eetimes, Sept 2026). [independent] [secondary]
- IEEE 802.3dj (200G/lane, covering 200G/400G/800G/1.6T) on track for completion late 2026; early 200G/lane products expected during 2026; 400G/lane project next. [independent] (networkworld)
- **DCI / scale-across:** emerging as a distinct revenue layer — NVIDIA Spectrum-XGS (available now, SW/FW upgrade), Cisco P200 silicon for scale-across, Arista 1.6T scale-across platforms. [official] [vendor-reported]
- **3.2T:** development underway around 400G-per-lane designs (Kozlov, eetimes); Spectrum-X3200/Quantum-X3200 on NVIDIA's roadmap (TrendForce). [secondary]

---

## 9. Key uncertainties and explicit flags

1. **Arista FY2026 guide conflict:** $11.5B (Aug 2026 coverage) vs $12.6B (remio.ai, Sept 19, 2026). Unresolved. [unverified]
2. **UEC 1.0.3 currency:** the "1.0.3 (July 16, 2026) current" claim comes from a community wiki, not ultraethernet.org. Verify before citing. [unverified]
3. **UEC production adoption:** no confirmed hyperscaler production deployment of UEC/UET found; 2026 evidence is interop demos, NICs, switches, test gear. [unverified]
4. **NVIDIA silicon-photonics/CPO shipping:** roadmap language (Spectrum-X Ethernet Photonics H2 2026; Quantum-X early 2026) vs SemiAnalysis yield concerns; Shainer's "shipping and ramping H2 2026" is a vendor claim. Large-scale CPO volume realistically 2028–2030 per Yole. [unverified]
5. **Quantum-3:** no evidence located; roadmap shows Quantum-2 → Quantum-X800 → Quantum-X1600. Do not assert a Quantum-3 product. [unverified]
6. **Spectrum-XGS announcement date:** "available now" release; Hot Chips mention suggests Sept 2025 but date not verified from NVIDIA's page. [unverified]
7. **Market-size figures conflict:** transceiver TAM estimates differ widely by firm (LightCounting ~$23.8B 2025 vs MarketsandMarkets ~$9.2B DC 2025). Treat as directional. [secondary]
8. **DriveNets claims** (6% better than IB, 15% better than Spectrum-X on NCCL): single vendor blog using SemiAnalysis test data, unaudited. [vendor-reported]
9. **Quantum-X800 latency/bandwidth table figures** (ascentoptics: <100 ns, 115.2 Tb/s vs 51.2 Tb/s): secondary product-page comparison, internally inconsistent with other latency quotes; quote with caution. [secondary]

---

## 10. Sources (URLs as found)

- https://investor.nvidia.com/news/press-release-details/2025/NVIDIA-Introduces-Spectrum-XGS-Ethernet-to-Connect-Distributed-Data-Centers-Into-Giga-Scale-AI-Super-Factories/default.aspx [official]
- http://nvidianews.nvidia.com/news/spectrum-x-ethernet-networking-xai-colossus [official]
- https://cxotoday.com/hardware/nvidia-debuts-spectrum-6-switches-to-power-ai-networks-of-the-future/ [vendor-reported]
- https://www.techpowerup.com/340218/nvidia-links-data-centers-into-a-unified-supercomputer-with-spectrum-xgs-ethernet [secondary]
- https://www.techpowerup.com/337615/broadcom-ships-tomahawk-6-switch-chip-series-with-102-4-tbps [secondary]
- https://pr.wvcjournal.com/article/Broadcom-Now-Shipping-Worlds-First-1024-Tbps-Switch-in-Production-Volume/69b31c1b3971457808b0979e [official] (GlobeNewswire)
- https://www.stocktitan.net/news/AVGO/broadcom-now-shipping-world-s-first-102-4-tbps-switch-in-production-abi415f14rou.html [secondary]
- https://temperature2.com/p/2026-09-06-guide-infiniband-vs-ethernet/ [secondary] (Sept 2026, independent-ish analysis)
- http://gpusmith.com/articles/en/pdfs/infiniband-vs-spectrum-x-vs-roce-vs-ethernet-ai-clusters.pdf [secondary] (Dell'Oro Q1 2026 data)
- https://rdp.in/gpu-mart/knowledge-base/infiniband-vs-spectrum-x-vs-ethernet-for-ai-clusters/ [secondary]
- https://ascentoptics.com/blog/nvidia-quantum-x800/ [secondary]
- https://www.trendforce.com/insights/infiniband-vs-ethernet [secondary] (TrendForce roadmap table)
- https://www.communicationstoday.co.in/over-800g-optical-transceiver-shipments-to-soar-2-6x-by-2026/ [secondary] (TrendForce figures)
- https://www.eetimes.com/ai-demand-reshapes-optical-connectivity-and-photonics-roadmaps/ [independent] (Kozlov commentary, Sept 2026)
- https://markets.financialcontent.com/kelownadailycourier/article/bizwire-2026-3-16-keysight-advances-ai-networking-with-ultra-ethernet-llr-and-cbfc-interoperability-demonstration-at-ofc-2026 [official] (Keysight)
- https://github.com/abuabdurahman82/llm-systems-wiki/blob/HEAD/AI-Factory-Networking/30-ultra-ethernet-consortium.md [secondary] (community wiki, 2026-08-26)
- https://www.networkworld.com/article/4113364/ethernet-groups-keep-2026-focus-on-higher-bandwidth-ai-demands.html [independent]
- https://itbrief.co.nz/story/semtech-sets-200g-lpo-targets-for-data-centre-links [independent] (Semtech LPO targets)
- https://dataintelo.com/report/linear-pluggable-optics-lpo-market [secondary] (market research estimate)
- https://www.eedesignit.com/why-hpc-chip-designers-are-turning-to-linear-pluggable-optics/ [secondary]
- https://www.gminsights.com/industry-analysis/optical-transceiver-market [secondary] (market research estimate)
- https://markets.financialcontent.com/pentictonherald/article/marketminute-2026-3-25-the-16t-supercycle-optical-and-networking-stocks-surge-as-hyperscalers-ignite-gigascale-ai-infrastructure [secondary]
- https://s21.q4cdn.com/861911615/files/doc_news/Arista-Networks-Inc--Reports-Second-Quarter-2026-Financial-Results-2026.pdf [official] (Arista Q2 2026)
- https://www.gurufocus.com/news/9003286/arista-networks-inc-reports-second-quarter-2026-financial-results [secondary]
- https://cryptobriefing.com/arista-networks-3b-quarter-ai-demand/ [secondary]
- https://www.tikr.com/blog/arista-networks-just-posted-its-first-3-billion-quarter-the-ai-networking-buildout-is-accelerating [secondary]
- https://www.zacks.com/commentary/2990188/bull-of-the-day-arista-networks-inc-anet [secondary] (Sept 16, 2026)
- https://www.remio.ai/post/arista-networks-outlook-jumps-as-ai-demand-meets-an-nvidia-challenge [secondary] (Sept 19, 2026 — $12.6B claim, conflicting)
- https://infotechlead.com/networking/cisco-revenue-jumps-18-to-17-3-bn-as-ai-orders-hit-9-3-bn-and-networking-demand-surges-97684 [secondary] (Cisco FY2026)
- https://www.ainvest.com/news/cisco-delivered-ai-infrastructure-results-wanted-margins-telling-2608/ [secondary]
- https://www.ainvest.com/news/cisco-ai-supercycle-architecturally-real-drop-earnings-wrong-read-2608/ [secondary]
- https://financefeeds.com/cisco-booked-4-billion-of-ai-orders-in-one-quarter-beat-revenue-and-eps-and-the-stock-still-fell/ [secondary]
- https://www.trefis.com/stock/csco/articles/615687/is-cisco-stock-priced-for-orders-that-are-not-yet-revenue/2026-09-17 [secondary] (Sept 17, 2026)
- https://www.techradar.com/pro/xais-colossus-supercomputer-cluster-uses-100-000-nvidia-hopper-gpus-and-it-was-all-made-possible-using-nvidias-spectrum-x-ethernet-networking-platform [secondary]
- https://www.sdxcentral.com/news/xai-to-double-colossus-compute-capacity-reveals-cluster-uses-nvidia-spectrum-x-ethernet/ [independent]
- https://measuredai.substack.com/p/xai-colossus-data-center-cluster [secondary]
- https://www.ainvest.com/news/arista-networks-anet-ai-driven-data-center-infrastructure-playbook-2026-2512/ [secondary]
- https://www.fxempire.com/forecasts/article/arista-networks-huge-ai-sales-upcoming-earnings-lift-shares-1614084 [secondary]
- https://github.com/47tzp4ydc9-cmyk/life-os/blob/HEAD/investment-os/narrative/research/silicon-photonics.md [secondary] (investor research note, 2026-07-02)
- https://github.com/roachx92/equity-watch/blob/HEAD/tickers/AAOI/reports/2026-07-14.md [secondary]
- https://medium.com/teradata-labs/the-ultra-ethernet-endgame-cec7242a243b [secondary] (advocacy piece)
- https://drivenets.com/blog/why-infiniband-falls-short-of-ethernet-for-ai-networking/ [vendor-reported]


---

## §4 AI server market figures

# AI Server Market Figures (2026) — Research Draft
**As of: September 22, 2026** | Language: English | Status: draft (étape 5)
**Coverage:** AI server market size, vendor shares, ODM share, GPU/general split, regional breakdowns, analyst reports, supply-chain notes.

**Provenance legend** (applied to every claim):
- `[official]` — vendor press releases / earnings disclosures / official analyst press releases
- `[vendor-reported]` — figures from vendor executives or investor materials
- `[independent]` — reputable third-party press/research interpretation of tracker data
- `[secondary]` — syndicated research firms / aggregators / trade press citing others
- `[unverified]` — blogs, LinkedIn posts, AI-generated summaries; treat with caution

> All figures are cited to their source as found in September 2026. Absolute dollar/unit totals differ sharply between sources because of **scope differences** (AI servers only vs. AI infrastructure vs. total server market; revenue vs. shipments; vendor revenue vs. end-user spend). See "Conflicting estimates" section.

---

## 1. Market size and growth — headline figures

### 1.1 TrendForce — AI server shipments and revenue
TrendForce's public AI-server forecasts evolved through 2025–2026 (each later release supersedes the earlier one):

- **October 30, 2025** press release [official]: [TrendForce](https://www.trendforce.com/presscenter/news/20251030-12762.html)
  - 2025: AI server shipment growth forecast "about 24%" (slightly lowered due to restrictions on NVIDIA H20 shipments to China and minor delays in GB300/B300 platform launches). [official]
  - 2025: AI server **revenue +~48% YoY**, driven by Blackwell platforms (GB200, GB300 full-rack solutions). [official]
  - 2026: AI server shipments to grow **"more than 20%"**; AI servers to account for **17% of overall server shipments**. [official]
  - 2026: AI server revenue to grow **"more than 30%"** and account for **74% of the overall server market value**. [official]
  - 2025: NVIDIA to maintain **~70% of the AI chip market share**. [official]
  - By 2026, ASIC shipments "expected to surpass those of GPUs," gradually reducing NVIDIA's share. [official] *(⚠️ tension with TrendForce's Jan-2026 figure that GPUs = 69.7% of AI server shipments — flagged in Conflicts.)*
  - HBM consumption expected to surge **70% in 2026**. [official]

- **January 20, 2026** press release [official]: [TrendForce](https://www.trendforce.com/presscenter/news/20260120-12887.html)
  - 2026: global AI server shipments forecast **>28% YoY**, outpacing overall server market growth of 12.8%. [official]
  - 2026: total global server shipments +12.8% YoY (AI inference boosting general-purpose server demand). [official]
  - In 2026, **GPUs remain the leading category at 69.7% of AI server shipments**; NVIDIA GB300-based systems expected to drive most shipments; VR200-based platforms to ramp gradually in 2H 2026. [official]
  - **ASIC-based AI servers: ~27.8% of shipments in 2026**, "the highest since 2023," with ASIC shipment growth outpacing GPU-based systems; Google investing more in own ASICs than most CSP competitors, positioned as a leading market player. [official]

- **April 15, 2026** press release (server industry, component constraints) [official]: [TrendForce](https://www.trendforce.com/presscenter/news/20260415-13013.html)
  - 2026 AI server shipment growth held at **~28% YoY**, ASIC-based systems expected to outpace GPU-based systems. [official]
  - ASIC share slightly revised down from nearly 28% to **~27%** (validation/tuning delays at players such as Meta and AWS). [official]
  - **Full-year 2026 total server shipment growth cut to ~13% YoY** (previously approaching 20%), because suppliers prioritize capacity for higher-margin AI server products and general-server component lead times lengthened. [official]

- **August 3, 2026** press release [official]: [TrendForce](https://www.trendforce.com/presscenter/news/20260803-13161.html)
  - 2026 AI server shipment forecast **revised upward from 28% to nearly 31% YoY**. [official]
  - Combined CapEx of the world's nine largest CSPs (Google, Amazon, Meta, Microsoft, Oracle, ByteDance, Tencent, Alibaba, Baidu) to rise **~90% YoY in 2026 to >US$886.7 billion**, with the five North American hyperscalers accounting for nearly 90%. [official]
  - 2026: major CSPs forecast to account for **76% of annual high-end AI server shipments**. [official] *(per TrendForce landing page: "By 2026, major CSPs are forecasted to account for 76% of the annual total shipments of high-end AI servers.")* [official] — https://www.trendforce.com/landingPage/ai_server
  - CSP AI-chip strategies 2026: Google expanding in-house TPUs through 2026–2027; AWS deploying NVIDIA GB300 as primary GPU platform in 2026 while expanding internal ASICs; Meta on NVIDIA GB/VR and AMD Helios rack-scale systems in 2026, accelerating proprietary AI ASICs in 2027. [official]

### 1.2 IDC — server tracker + AI infrastructure tracker
IDC's Worldwide Quarterly Server Tracker (vendor revenue, not units) — figures from IDC's own press releases and IDC-quoted reporting:

- **Q1 2026** [official]: [IDC press release, June 15, 2026](https://www.idc.com/resource-center/press-releases/1q26-server-tracker/)
  - Worldwide server market: **$122.6B vendor revenue, +30.4% YoY** (vs. $94.1B in 1Q25). [official]
  - GPU-accelerated servers: **$68.9B (+24.8% YoY), 56.2% of total market revenue**. [official]
  - "Other Accelerated" servers: **$17.7B (+122.1% YoY)**. [official]
  - Non-x86 servers: **$58.7B (+107.6% YoY), 47.9% of total revenue**. [official]
  - x86 servers: **$63.9B (−2.9% YoY)**. [official]
  - ODM Direct revenue share compressed from **64.1% (1Q25) to 50.2% (1Q26)**. [official]
  - IDC: "supply — not demand — is the primary ceiling on near-term server market growth"; component constraints (DRAM, NAND) limiting non-accelerated volumes. [official]

- **Q2 2026** [independent] (IDC tracker via StorageReview, published ~Sept 21, 2026): [StorageReview](https://www.storagereview.com/news/idc-external-storage-tracker-q2-2026-10-3-billion-as-server-market-tops-166-billion)
  - Worldwide server market: **$166.3B vendor revenue, +52.0% YoY and +35.7% QoQ** — highest quarterly total in tracker history, surpassing the previous record of **$125.3B set in Q4 2025**. [independent]
  - GPU-accelerated servers: **$87.4B (+28.1% YoY), 52.6% of total revenue**. [independent]
  - "Other Accelerated" servers: **$27.5B (+237.6% YoY)**. [independent]
  - Non-x86 servers: **$74.4B (+146.0% YoY), 44.8% of total revenue**. [independent]
  - x86 servers: **$91.9B (+16.1% YoY)**. [independent]
  - Worldwide unit shipments: **+15.4% YoY** (revenue +52% ⇒ both volume resumption and ASP increases). [independent]
  - GPU-accelerated server ASP rose from **~$118.6K to ~$170.2K (+43.6% YoY)** even as GPU server unit shipments **fell 10.8% YoY**. [independent]
  - Non-accelerated server ASP rose from ~$9,800 to ~$13,000 (+33.5% YoY) alongside +16.7% unit growth. [independent]
  - IDC expects component costs to stay elevated "well into 2027," keeping pricing-driven growth in place. [independent]

- IDC Worldwide Quarterly **AI Infrastructure** Tracker (compute + storage hardware), published **July 21, 2026** [secondary] (via QuantumRun summary): [QuantumRun](https://www.quantumrun.com/consulting/global-ai-market-size/)
  - Full-year AI infrastructure spending: 2024: **$153B (+107.6%)**; 2025: **$318B**; 2026 (forecast): **$497B (~+56%)**; 2029: **$1.08T**; 2030: **$1.21T**. [secondary]
  - Five-year CAGR ~30% from 2025; the 2026 forecast was revised **up** from the 53% pace estimated a quarter earlier. [secondary]
  - Servers accounted for **97.6% of Q1 2026 AI infrastructure value ($87.6B vs. $2.2B for storage)**. [secondary]
  - Within accelerated servers, "ARM rack-scale platforms passed x86" (per IDC research director Juan Seminara — an "architecture shift rather than demand destruction"). [secondary]
  - **Regional split, Q1 2026**: United States **$67.9B (75.7% of global spend, +30.3% YoY)**; China returned to growth at **$7.8B**. [secondary]

- IDC full-year server market 2025 [secondary] (via Data Center Knowledge, ~Sept 2026): [Data Center Knowledge](https://www.datacenterknowledge.com/servers/ai-server-market-update-vendors-shift-from-silicon-to-services)
  - "IDC reports the global server market reached a record **$444 billion in 2025**." [secondary]
  - "IDC projects AI infrastructure spending will reach **$487 billion in 2026** and surpass **$1 trillion by 2029**." [secondary] *(⚠️ conflicts with the $497B 2026 figure from the July-2026 tracker vintage — likely a different/earlier tracker vintage; flagged in Conflicts.)*
  - GPU availability "still constrained," memory shortages driving costs. [secondary]

### 1.3 Gartner — IT/data-center spending forecasts
- **February 2026** forecast [secondary, widely reported from Gartner press materials]: [HPCwire](https://www.hpcwire.com/aiwire/2026/02/04/gartner-forecasts-worldwide-it-spending-to-grow-10-8-in-2026-totaling-6-15t/)
  - Worldwide IT spending 2026: **$6.15T, +10.8%** (vs. $5.555T in 2025). [secondary]
  - Data center systems: **$653.4B, +31.7%** (vs. $496.2B, +48.9% in 2025). [secondary]
  - **Server spending +36.9% YoY**, "driven almost entirely by AI-optimized hardware"; hyperscaler demand cited by Gartner VP analyst John-David Lovelock. [secondary]
- **July 2026** forecast revision [secondary]: [TechBriefly](https://techbriefly.com/2026/07/28/2026-global-it-spending-forecast-6-37-trillion/)
  - Worldwide IT spending 2026 revised to **$6.37T (+14.2%)** (up from $6.15T in Feb and $6.31T in April). [secondary]
  - Data center systems: **$822B, +62.5%** (per Business Standard reporting of the forecast). [secondary]
  - Gartner May 2026: worldwide **AI spending $2.59T in 2026, +47%**, "with most of that spending directed toward servers, accelerators, storage and data center platforms rather than applications." [secondary]

### 1.4 Omdia
Omdia published no widely cited AI-server-specific 2026 market-size figure in the sources surveyed; relevant 2026 datapoints:
- Omdia raised its 2026 **semiconductor** revenue forecast to **+94.1% YoY** (July 30, 2026, via Business Wire) [secondary]: memory ICs expected to account for **>50% of total semiconductor revenue in 2026**; DRAM to nearly double in value vs. 2025; NAND to ~4× 2025 level; computing & data storage semiconductor revenue +90% YoY to **>$700B**. Bottlenecks across HBM, advanced packaging, and node capacity expected to persist until at least 2027. [Markets/FinancialContent](https://markets.financialcontent.com/fatpitch.valueinvestingnews/article/bizwire-2026-7-30-omdia-ai-demand-drives-941-surge-in-semiconductor-forecast-for-2026) [secondary]
- Omdia attributes memory imbalance partly to industry focus on HBM (lower volumes, higher price points, reducing conventional memory capacity); supply relief limited "well into 2027." [secondary] — [Evertiq](https://EVERTIQ.COM/news/2026-04-28-omdia-lifts-2026-semiconductor-outlook-as-ai-driven-memory-constraints-intensify)

### 1.5 Syndicated/secondary market-size estimates (use with caution)
Scope definitions vary and growth rates are far lower than IDC/TrendForce trackers; included for completeness, tagged `[secondary]`:

- 360iResearch (June 2026 report): AI server market 2025: **$148.43B** → 2026: **$169.18B** → 2032: **$400.93B**, 15.25% CAGR. [360iResearch](https://www.360iresearch.com/library/intelligence/artificial-intelligence-server) [secondary]
- Research Nester: 2025: **$169.81B** → 2026: **$223.61B** → 2035: **$3.47T**, 35.2% CAGR 2026–2035; North America largest (31.6% share by 2035), APAC fastest-growing. [Research Nester](https://www.researchnester.com/reports/ai-server-market/6856) [secondary]
- Grand View Research (via Sept 2026 press-release site): AI server market **$131.7B in 2025** → **$157.0B in 2026** → $598.1B by 2033 (21.1% CAGR); GPU-based servers >53% of 2025 revenue. [express-press-release](https://express-press-release.net/news/2026/09/09/1771179) [secondary]
- Global Market Insights (via July 2026 compilation): AI server market valued at **$128B in 2024**. [Araziyah PDF](http://araziyahsafety.co.za/araziyah/files/AI-Server-Revenue-Situation---Araziyah-Safety-Infrastructure-(Pty)-Ltd_Fri-03-Jul-2026-16133.pdf) [secondary]

### 1.6 AI server unit volumes (industry estimates)
- 2023: ~**1.2M AI server units** shipped (+38.4% YoY), ~9% of total server shipments (TrendForce, 2023). [secondary — via archived TrendForce summary](https://markets.financialcontent.com/theeveningleader/article/bizwire-2023-5-30-global-ai-server-shipments-forecasted-to-increase-40-in-2023-amid-rising-ai-demand-says-trendforce)
- 2025→2026: ~**2.15M units (2025) → ~2.75M units (2026), ~+28% YoY** (industry analysis citing market forecasts). [wnie.online](https://www.wnie.online/ai-servers-are-reshaping-the-entire-electronics-supply-chain/) [secondary]
- TrendForce long-run: AI server shipments CAGR **27.2% (2022–2027)**; AI servers ~**19% of total annual server shipments by 2027** (from ~9% in 2023). [secondary — via invest-bud compilation](http://invest-bud.com.pl/invest-bud/files/Global-AI-Server-Shipments_Wed-14-Aug-2024-16264.pdf)
- IDC Q2 2026: **~5.16M total server units** shipped in the quarter (Next Platform's arithmetic on IDC data); accelerated (GPU+XPU) systems ~**16.1% of units but 69.1% of revenue**. [independent] — [Next Platform, Sept 21, 2026](https://www.nextplatform.com/compute/2026/09/21/accelerated-server-sales-are-themselves-accelerating/5297993)
- NVIDIA AI server **rack** shipments (Taiwan brokerage estimates via BigGo Finance, May 2026): **75,000 racks in 2026** → 100,000–110,000 in 2027. [secondary] — [BigGo Finance](https://finance.biggo.com/news/sc6GJZ4BDXrLZJaA9J06)
- Google **TPU rack** shipments (same source): **60,000 in 2026 → 105,000 in 2027 (+75%)**. [secondary]

---

## 2. Vendor market shares

### 2.1 IDC Worldwide Quarterly Server Tracker — overall server vendor revenue share
(IDCs tracker covers all servers; accelerated/AI servers drive the growth but IDC does not publish a separate public AI-server-only vendor ranking. Shares below are *overall server revenue* shares — heavily AI-driven in 2026.)

**Q1 2026** [official] — [IDC](https://www.idc.com/resource-center/press-releases/1q26-server-tracker/):
| Rank | Vendor | Revenue | Share | YoY |
|---|---|---|---|---|
| 1 | Dell Technologies | $20.28B | **16.5%** | +244.1% |
| 2 | Super Micro | $9.33B | **7.6%** | +128.9% |
| 3 | Lenovo | $5.62B | **4.6%** | +36.5% |
| 4 | IEIT Systems | $4.01B | 3.3% | −7.0% |
| 5 | HPE | $3.72B | 3.0% | +17.2% |
| — | ODM Direct | $61.54B | 50.2% | +2.1% (share down from 64.1% in 1Q25) |
| — | Rest of market | $18.11B | 14.8% | +48.3% |

**Q2 2026** [independent] (IDC tracker via StorageReview):
| Rank | Vendor | Revenue | Share | YoY |
|---|---|---|---|---|
| 1 | Dell Technologies | $22,240.6M | **13.4%** | +165.4% |
| 2 | Supermicro | $10,177.0M | **6.1%** | +97.2% |
| 3 | Lenovo | $8,411.3M | **5.1%** | +99.6% |
| 4 | HPE | $5,868.4M | **3.5%** | +46.0% |
| 5 | IEIT Systems | $4,001.5M | 2.4% | −8.1% |
| — | ODM Direct | $89,660.3M | **53.9%** | +35.2% (share down from 60.6% in 2Q25) |
| — | Rest of market | $25,960.1M | 15.6% | +53.4% |
| — | **Total** | $166,319.2M | 100% | +52.0% |

[StorageReview](https://www.storagereview.com/news/idc-external-storage-tracker-q2-2026-10-3-billion-as-server-market-tops-166-billion) [independent]

**Q4 2025** (reference, via ARN ~March 2026 reporting IDC) [secondary] — [ARN](https://www.arnnet.com.au/article/4148995/idc-dell-leads-server-market-driven-by-ai-infrastructure-needs-3.html):
- Supermicro: **$11.7B, 9.5% share** (triple-digit growth on accelerated servers).
- IEIT Systems and Lenovo statistically tied: 4.1% / 4.0%.
- HPE: **$3.8B, 3.1% share** (−8.6% YoY); IDC's Lidice Fernandez attributed the decline to HPE repositioning around edge, hybrid IT, and mission-critical systems rather than competing in high-volume x86.
- Dell #1 on accelerated servers (share figure not captured in the excerpt).

### 2.2 AI-server-specific vendor signals (earnings and company disclosures)
No public IDC/TrendForce AI-server-only vendor ranking was found in open sources; the following are vendor-reported AI-revenue datapoints:

- **Dell Technologies — Q2 FY2027** (reported Sept 1–2, 2026; quarter ended ~Aug 2026) [vendor-reported]:
  - Record revenue **$47.0B (+58% YoY)**; non-GAAP EPS $7.04 (+203%). [Motley Fool transcript summary](https://www.fool.com/earnings/call-transcripts/2026/09/08/dell-dell-q2-2027-earnings-call-transcript/) [independent]
  - AI server orders: **$60.9B** (record quarter); AI server revenue recognized: **$16.4B** (record); AI backlog: **$95B** (record ≈ 5.8 quarters of run-rate). [independent]
  - Full-year FY27 guidance: revenue **$192B** (~+70% YoY, raised $25B); **AI server revenue to triple to $74B**. [independent]
  - AI customer count >6,500 (+3,300 new in last 3 quarters); growth "disproportionately from enterprise customers." [independent] — [BigGo Finance](https://finance.biggo.com/news/US_DELL_2026-09-08)
  - Traditional servers & networking: $10.5B (+122% YoY); storage $4.9B (+26%). [independent]
  - Memory component availability cited as the **primary constraint limiting total shipment volume**. [independent]
  - Earlier: Q4 FY2026 (reported Feb 2026): AI-optimized server revenue **$9B (+342% YoY)**, AI backlog **$43B** [secondary] — [Data Center Knowledge](https://www.datacenterknowledge.com/servers/ai-server-market-update-vendors-shift-from-silicon-to-services). *(A July-2026 compilation described the same FY2026-Q4 print as +39% total sales with AI server revenue "expected to double next year" — conflicting framing, flagged.)*

- **Supermicro — Q4 FY2026** (ended June 30, 2026; reported Aug 11, 2026) [vendor-reported/secondary]:
  - Q4 revenue **$11.12B** (vs. ~$11.55B consensus); FY2026 revenue **$39.1B (+78% YoY)**. [secondary] — [BigGo Finance](https://finance.biggo.com/news/936a3b6a-b9f9-4449-ac06-5d3c39b96a4e)
  - **>$60B in new orders** during Q4; record backlog entering FY27. [secondary]
  - FY2027 revenue guidance: **$65–72B** (consensus ~$54.4B); Q1 FY27 guide $14.5–15.5B. [secondary]
  - AI GPU-related platforms: **>80% of fiscal Q3 FY2026 revenue** (May 2026 report). [secondary]
  - Q4 FY26 customer mix: enterprise & channel $5.6B (50%, +172% YoY), OEM & large data-center $5.5B (50%, +50% YoY) — mix shift toward enterprise credited for Q4 gross-margin rebound to **17.5%** (guided 10.4–10.8% for Q1 FY27). [secondary] — [ainvest](https://www.ainvest.com/news/supermicro-turnaround-finally-sense-selling-hyperscalers-lifted-q4-margin-17-5-2608/), [BigGo](https://finance.biggo.com/news/a3f6f76c-f05e-4728-aeb5-d011d5cc59af)
  - CEO Charles Liang attributed part of Q4 revenue timing to customer delays around power, cooling, and networking infrastructure. [secondary]

- **Lenovo — Q3 FY2025/26** (reported Feb 12, 2026) [vendor-reported/secondary]:
  - High double-digit AI server revenue growth; AI server pipeline **~$15.5B**. [secondary] — [Data Center Knowledge](https://www.datacenterknowledge.com/servers/ai-server-market-update-vendors-shift-from-silicon-to-services)

- **HPE — Q1 FY2026** (reported March 9, 2026) [vendor-reported/secondary]:
  - Server revenue of **$4.2B** within Cloud & AI segment; **$5B AI systems backlog**, primarily enterprise and sovereign AI customers. [secondary] — [Data Center Knowledge](https://www.datacenterknowledge.com/servers/ai-server-market-update-vendors-shift-from-silicon-to-services)

- **Giga Computing (Gigabyte)** — listed among task-relevant vendors; no 2026 share figure found in open sources. *(Gap noted — TrendForce/DIGITIMES paywalled reports likely carry it.)*

---

## 3. ODM / white-box share in AI servers

### 3.1 IDC "ODM Direct" (aggregate)
- Q1 2026: **$61.54B revenue, 50.2% share** (+2.1% YoY; share down from 64.1% in 1Q25) — branded OEMs captured a growing share of AI infrastructure deployments. [official — IDC]
- Q2 2026: **$89.66B revenue, 53.9% share** (+35.2% YoY; share down from 60.6% in 2Q25) — still the dominant absolute position, but share compressing as branded OEMs grew faster. [independent — StorageReview on IDC]
- LinkedIn analysis of IDC's Q2 2026 data notes the share compression reflects OEM gains in AI infrastructure projects; hyperscaler direct racks remain the ODM Direct base. [unverified — LinkedIn](https://www.linkedin.com/pulse/global-server-market-hits-record-1663b-q2-2026-ai-entering-bruce-zhou-fdzuc)

### 3.2 Foxconn (Hon Hai) — AI server share claims
- Chairman Young Liu (earnings, March 2026): Foxconn holds **"around a 40 per cent share of the global AI server market"**; AI server rack shipments to **double in 2026**; company issued first-ever full-year outlook at its highest guidance tier ("strong growth"); US AI server production hub expected to roll out **2,000 AI server racks/week in 2026**. [vendor-reported — via Focus Taiwan/ANI] — [NewsPoint](https://www.newspointapp.com/english/business/foxconn-expects-high-double-digit-growth-in-ai-server-rack-shipments-in-q1-sees-shipments-doubling-in-2026-aninews/articleshow/14504820f21cd6239137461aa62fda4e0191c7f5)
- Taiwan brokerages (May 2026): Foxconn projected to capture **40% of NVIDIA AI server racks in 2026 and 50% in 2027**. [secondary] — [BigGo Finance](https://finance.biggo.com/news/sc6GJZ4BDXrLZJaA9J06)
- Foxconn Q1 2026: revenue **NT$2.1195T (+29% YoY, record quarter)**; net profit NT$49.9B (+19%); cloud & networking (incl. AI servers) **~50% of total revenue**. [vendor-reported — via BigGo] — [BigGo Finance](https://finance.biggo.com/news/TW_2382.TW_2026-05-14)
- June 2026 revenue alone: ~NT$1.33T (~$45B), +40% YoY (per industry write-up citing thenextweb). [secondary] — [GPUSmith PDF](http://gpusmith.com/articles/en/pdfs/nvidia-ai-server-oem-comparison.pdf)
- Market researchers "expect Foxconn's AI server market share to approach 40% in 2026" [secondary] — [AICerts](https://www.aicerts.ai/news/foxconn-rides-ai-server-demand-to-record-profits/). *Note: Foxconn's own "40%" claim and analysts' "approach 40%" are not necessarily the same denominator (racks vs. revenue; NVIDIA racks vs. all AI servers).*

### 3.3 Quanta
- Q1 2026: consolidated revenue **NT$809.2B (+66.6% YoY, record)**; servers **>80% of total revenue**; **AI servers >75% of server revenue**; AI server shipments +30% QoQ; GB300 racks (unit price >$3M) now exceeding GB200 shipments; gross margin compressed to 4.78% on product-mix shift. [vendor-reported — via BigGo Finance] — [BigGo Finance](https://finance.biggo.com/news/TW_2382.TW_2026-05-14)
- Executives: major CSP order visibility **through 2028**. [secondary — via Taipei Times/GPUSmith](http://gpusmith.com/articles/en/pdfs/nvidia-ai-server-oem-comparison.pdf)

### 3.4 Wiwynn
- Q1 2026: consolidated revenue **NT$276.5B (+62.0% YoY)**, a record for the period. [vendor-reported — via wiwynn.com as cited in GPUSmith](http://gpusmith.com/articles/en/pdfs/nvidia-ai-server-oem-comparison.pdf)

### 3.5 Wistron / Inventec / Compal
- No 2026 AI-server revenue/share figures found in open sources; Compal chairman (Jan 2025) anticipated "significant surge in AI server shipments beginning in 2026" [secondary — DIGITIMES]. *(Gap noted.)*

### 3.6 DIGITIMES Research — white-label share
- White-label (datacenter) general-purpose and AI servers to account for **>50% of total server shipments in 2026**; white-label segment to grow steadily over the next five years (CAGR ~6% for total server shipments 2023–2028). [secondary — DIGITIMES report summary] — [DIGITIMES](https://www.digitimes.com/reports/item.php?id=20240227RS400)
- DIGITIMES Research (Oct 2025 report): global server shipments CAGR **~5% (2025–2030)**; growth to slow in 2026 before a 7.8% rebound in 2027. [secondary — via TechNewsTube summary](https://technewstube.com/digitimes/1772459/global-server-shipment-forecast-2026-beyond/)
- DIGITIMES (Feb 2026): AI demand spillover lifting **2026 general-purpose server shipments +10%**. [secondary — paywalled] — [DIGITIMES](https://www.digitimes.com/news/a20260206PD221/demand-shipments-2026-general-purpose-server-growth.html)

---

## 4. GPU server vs. general server split; 8-GPU platform notes

### 4.1 Accelerated vs. non-accelerated (IDC)
- Q1 2026: GPU-accelerated **56.2% of server revenue** ($68.9B); other accelerated 14.4% ($17.7B); combined accelerated ≈ **70.6% of revenue**. [official — IDC]
- Q2 2026: GPU-accelerated **52.6%** ($87.4B); other accelerated 16.5% ($27.5B); combined ≈ **69.1% of revenue**. [independent — StorageReview on IDC]
- Q2 2026 units: accelerated systems ≈ **16.1% of units**, non-accelerated ≈ **83.9%** (~4M+ units, +16.7% YoY, $51.42B revenue, +55.8% YoY) — Next Platform's derivation from IDC data. [independent]
- Next Platform (Sept 21, 2026) estimates ~**840,000 GPU/XPU systems** shipped in Q2 2026 at an average ~$140K–$163K, and ~**325,000 XPU (non-GPU accelerator) systems** at ~$85K ASP; XPU system shipments estimated up 2.4× YoY with ASPs up ~45%. [independent] *(⚠️ its internal arithmetic — $140K × 840K ≠ the $137.35B total it states — is inconsistent; treat unit/ASP figures as estimates.)* — [Next Platform](https://www.nextplatform.com/compute/2026/09/21/accelerated-server-sales-are-themselves-accelerating/5297993)

### 4.2 TrendForce — GPU vs. ASIC vs. general
- 2026: GPUs **69.7% of AI server shipments**; ASIC-based AI servers ~27–27.8% (revised slightly down mid-year); remainder FPGA/other. [official — TrendForce, Jan/Apr 2026]
- AI inference servers projected at **nearly 50% of total AI server shipments** (TrendForce, older view via InfotechLead). [secondary]
- Inference growth is lifting general-purpose server shipments (DIGITIMES: +10% in 2026) as inference pre/post-processing runs on general servers, not just AI racks. [secondary]

### 4.3 8-GPU platforms — pricing and volume signals
- **B200 8-GPU configurations**: NVIDIA list price ~**$30,000–$40,000 per GPU** in 8-GPU volume; 2026 street quotes under supply constraints: **$45,000–$55,000/unit**. [unverified — tech-insider.org citing Silicon Data] — [tech-insider.org](https://tech-insider.org/nvidia-b200-residual-value-158-percent-2026/)
- DGX B200 appliance: ~**$515,000** list; 8-GPU HGX B200 server: **$400,000–$500,000** (quotes commonly ~$450,000). [unverified]
- **B200 residual value: 158% of launch price** ~1 year after mass availability (Silicon Data, reported Sept 16, 2026) — scarcity premium; A100/H100 also holding above straight-line depreciation. [unverified]
- Cloud B200 rental: neo-cloud on-demand averaged **$5.09/GPU-hour** in Q2 2026; hyperscaler list up to **$14.24/GPU-hour** (AWS Blackwell). [unverified]
- AMD AI GPU units (UBS Global Research, Feb 2026): 2025 revised +28% to **544,500 units**; 2026 revised +59% to **816,800 units** (MI350-led); 2027 ~**1.9M units** (MI450 ramp). [secondary — via Wccftech] — [Wccftech](https://wccftech.com/amd-server-cpu-revenue-to-surge-80-percent-2026-estimated-1-9m-ai-gpus-shipping-by-2027/)
- Rack-scale platforms (GB300 NVL72): **$3–4.5M per rack** (72 Blackwell Ultra GPUs, 36 Grace CPUs, 576 HBM3E stacks, NVLink switching, liquid cooling). [secondary — wnie.online]
- Small-transaction datapoint: Quanome Technologies 8-K (Sept 21, 2026): 32 GPU server units from Compal for **$18.8M** (~$587K/unit), US data-center delivery. [official — SEC filing via Kalkine] — [Kalkine](https://kalkine.com/news/announcements/quanome-technologies-signs-188m-gpu-server-purchase-deal)

---

## 5. Regional breakdowns

### 5.1 IDC server revenue by region
**Q1 2026** [official — IDC](https://www.idc.com/resource-center/press-releases/1q26-server-tracker/):
- United States: **$79.6B (+24.1% YoY), 64.9% of global revenue**. [official]
- PRC (China): **$19.2B (+30.9% YoY)**. [official]
- APeJC: $9.7B (+45.2%); Western Europe: $7.6B (+80.6%); Japan: −16.1% (vs. strong 1Q25 base). [official]
- Canada +190.9%, Middle East & Africa +121.4%, Latin America +64.1%. [official]
- Sovereign AI programs span **>40 countries** — a new policy-driven demand layer. [official — per InfotechLead summary]

**Q2 2026** [independent — StorageReview on IDC](https://www.storagereview.com/news/idc-external-storage-tracker-q2-2026-10-3-billion-as-server-market-tops-166-billion):
- United States: **$112.2B (+54.9% YoY), 67.4% of global revenue**. [independent]
- PRC: **$26.4B (+43.4% YoY)**, "reaccelerating from recent quarters." [independent]
- APeJC: $10.9B (+31.5%); Western Europe: $9.1B (+62.7%); Central & Eastern Europe: $0.7B (+98.3% off small base); Japan +11.1%. [independent]
- Canada: **+202.6% YoY** (fastest-growing worldwide); Middle East & Africa +68.8%; Latin America +32.8%. [independent]

### 5.2 IDC AI infrastructure spending by region
- Q1 2026: United States **$67.9B (75.7% of global AI infrastructure spend, +30.3% YoY)**; China **$7.8B** (returned to growth). [secondary — QuantumRun on IDC AI Infrastructure Tracker]

### 5.3 China-specific signals
- NVIDIA H20 China shipment restrictions trimmed TrendForce's 2025 AI-server growth forecast (to ~24%). [official — TrendForce, Oct 2025]
- Jan 2026: US BIS shifted H200/MI325X-equivalent export licenses from "presumption of denial" to "case-by-case review"; ~10 Chinese firms (incl. Alibaba, Tencent, ByteDance) cleared to buy H200 with a **75,000-unit cap per customer** (per industry analysis). [unverified — Momoview]
- TrendForce (Aug 2026): Chinese CSPs accelerating deployment of domestic AI solutions for LLM services. [official]
- CXMT (ChangXin) has become the largest DRAM supplier outside Samsung/SK Hynix/Micron, though the top three still hold >80% combined share. [secondary — fiisual]

### 5.4 North America dominance in CapEx
- Nine largest CSPs' 2026 CapEx >$886.7B (+~90% YoY); **five North American hyperscalers ≈ 90%**. [official — TrendForce, Aug 2026]
- Top-five hyperscaler 2026 CapEx ≈ $685B (+~70% vs. ~$400B in 2025); 2027 forecast >$850B, some analysts >$1T. [secondary — wnie.online]
- "Big Tech combined AI-related outlays set to surpass $730B this year" (2026) per market commentary around Supermicro's guidance. [secondary — BigGo Finance]

---

## 6. Notable analyst reports and publications (2026)

| Date | Publisher | Title / content | Link | Provenance |
|---|---|---|---|---|
| Jan 20, 2026 | TrendForce | Global AI server shipments forecast >28% YoY in 2026, rising ASIC share | [presscenter/news/20260120-12887](https://www.trendforce.com/presscenter/news/20260120-12887.html) | [official] |
| Apr 15, 2026 | TrendForce | 2026 server shipments +13% YoY on component delays; AI servers ~28% | [presscenter/news/20260415-13013](https://www.trendforce.com/presscenter/news/20260415-13013.html) | [official] |
| Aug 3, 2026 | TrendForce | AI server shipments forecast raised to nearly 31% YoY; CSP CapEx +90% | [presscenter/news/20260803-13161](https://www.trendforce.com/presscenter/news/20260803-13161.html) | [official] |
| Oct 30, 2025 | TrendForce | >20% AI server shipment growth in 2026; revenue +30%+, 74% of server value | [presscenter/news/20251030-12762](https://www.trendforce.com/presscenter/news/20251030-12762.html) | [official] |
| Jun 15, 2026 | IDC | Worldwide Server Market Revenue Surpasses $122B in Q1 2026 | [idc.com](https://www.idc.com/resource-center/press-releases/1q26-server-tracker/) | [official] |
| Jul 21, 2026 | IDC | Worldwide Quarterly AI Infrastructure Tracker (2026 forecast $497B) — via summary | [quantumrun.com](https://www.quantumrun.com/consulting/global-ai-market-size/) | [secondary] |
| ~Sept 2026 | IDC | Q2 2026 server tracker: $166.3B record — via StorageReview | [storagereview.com](https://www.storagereview.com/news/idc-external-storage-tracker-q2-2026-10-3-billion-as-server-market-tops-166-billion) | [independent] |
| Feb 2026 | Gartner | IT spending $6.15T; data center systems $653.4B (+31.7%); servers +36.9% | [hpcwire.com](https://www.hpcwire.com/aiwire/2026/02/04/gartner-forecasts-worldwide-it-spending-to-grow-10-8-in-2026-totaling-6-15t/) | [secondary] |
| Jul 2026 | Gartner | IT spending revised to $6.37T; data center systems $822B (+62.5%) | [techbriefly.com](https://techbriefly.com/2026/07/28/2026-global-it-spending-forecast-6-37-trillion/) | [secondary] |
| May 2026 | Gartner | Worldwide AI spending $2.59T in 2026 (+47%) | via [techbriefly.com](https://techbriefly.com/2026/07/28/2026-global-it-spending-forecast-6-37-trillion/) | [secondary] |
| Jul 30, 2026 | Omdia | 2026 semiconductor forecast raised to +94.1% YoY on AI memory | [financialcontent.com](https://markets.financialcontent.com/fatpitch.valueinvestingnews/article/bizwire-2026-7-30-omdia-ai-demand-drives-941-surge-in-semiconductor-forecast-for-2026) | [secondary] |
| Oct 2025 | DIGITIMES Research | Global server shipment forecast 2026 and beyond (CAGR ~5% 2025–2030) | [digitimes.com](https://www.digitimes.com/reports/item.php?id=20240227RS400) (summary) | [secondary] |
| Feb 6, 2026 | DIGITIMES | AI spillover lifts 2026 general-purpose server shipments 10% (paywalled) | [digitimes.com](https://www.digitimes.com/news/a20260206PD221/demand-shipments-2026-general-purpose-server-growth.html) | [secondary] |
| Sept 21, 2026 | Next Platform | "Accelerated Server Sales Are Themselves Accelerating" (IDC Q2 analysis) | [nextplatform.com](https://www.nextplatform.com/compute/2026/09/21/accelerated-server-sales-are-themselves-accelerating/5297993) | [independent] |
| Sept 2026 | Data Center Knowledge | AI Server Market Update: Vendors Shift from Silicon to Services | [datacenterknowledge.com](https://www.datacenterknowledge.com/servers/ai-server-market-update-vendors-shift-from-silicon-to-services) | [secondary] |
| Jul 2026 | GPUSmith | NVIDIA AI Server OEM Comparison: Supermicro vs Dell vs HPE (PDF) | [gpusmith.com](http://gpusmith.com/articles/en/pdfs/nvidia-ai-server-oem-comparison.pdf) | [secondary] |

---

## 7. Supply chain notes 2026

### 7.1 GPU allocation and pricing
- IDC: "supply — not demand — is the primary ceiling on near-term server market growth" (Q1 2026). [official]
- B200 street pricing under 2026 supply constraints: **$45–55K/unit** in 8-GPU volumes vs. $30–40K list; resale/residual value at **158% of launch price** (Silicon Data, Sept 2026). [unverified]
- Dell (Sept 2026): memory component availability is the **primary constraint** limiting total shipment volume; high demand keeping backlog elevated through year-end. [independent — earnings coverage]
- Supermicro CEO (Aug 2026): some Q4 revenue pushed by customer delays tied to **power, cooling, and networking infrastructure** — "the next bottleneck in AI may not be compute itself." [secondary]
- AIwire/Gartner (Feb 2026): demand from hyperscalers for AI-optimized servers shows no sign of plateauing; AI infrastructure investment "moved from cyclical to durable." [secondary]

### 7.2 HBM and memory
- **All three HBM suppliers' 2026 capacity sold out** — booked by NVIDIA/AMD/Google TPU by end of Q1 2026; shortage forecast to persist **through 2028** (sell-side). [unverified — Momoview industry analysis]
- TrendForce: HBM consumption **+70% in 2026**; HBM3→HBM3E→HBM4 per-stack pricing ≈ **$200→$300→$500** (TrendForce/Silicon Analysts estimates). [official / unverified]
- TrendForce: SK Hynix holds **>50% of global HBM market**, Samsung and Micron ~25% each. [secondary — via Notebookcheck on TrendForce]
- Customer allocation (Q1 2026, industry analysis): SK Hynix ~70% of NVIDIA Rubin HBM4; Samsung >60% of Google TPU HBM3E + AMD MI455X HBM4 partnership; Micron #2 NVIDIA slot + dual-source on AMD MI350. [unverified]
- Server DRAM contract prices: **+90–95% in Q1 2026**; **+50–55% QoQ in Q2 2026** (TrendForce); enterprise NAND +55–60% in Q1 2026; 16GB server memory modules doubled in six months; high-end enterprise SSDs nearly doubled. [secondary]
- Commercial DRAM prices up **~700% since 2022** (TrendForce via fiisual). [secondary]
- Micron: 1 unit of HBM3E consumes ~**3× the wafer capacity** of the same bits in DDR5; TrendForce: 1GB HBM ≈ 4GB standard DRAM in wafer area; revenue per wafer for HBM runs 3–5× DDR5. [secondary]
- Fulfillment: DRAM suppliers meeting only **75–80% of demand in 2H 2026**, possibly ~60% in 2027 (Meritz Securities); Micron meeting **50–65%** of key customer requests (CEO Sanjay Mehrotra). [secondary]
- SK Hynix: entire 2026 HBM output sold; signed supply agreements with OpenAI for Stargate, expected to **more than double the industry's total HBM requirements**. [secondary — Notebookcheck]
- Gartner analyst Shrish Pant: HBM wafer reallocation "very real," impacting the market "till the end of 2027"; meaningful price relief unlikely before late 2027. [secondary — TechTimes/Computex 2026]
- Samsung memory chief (Apr 2026 earnings): "significant shortages" across memory products expected **through at least 2027**. [secondary — TechTimes]
- TSMC CoWoS capacity: ~35K wafer starts/month (late 2024) → **120–130K/month by end of 2026** (~4× in <2 years), "and demand still outpaces it." [secondary — TechTimes]

### 7.3 Component lead times
- TrendForce (Apr 2026): suppliers prioritize AI-server allocation; general-server component lead times **extending to nearly one year** [official]:
  - PMICs: 21–26 weeks → **35–40 weeks** (8-inch BCD capacity allocated to AI PMICs; Samsung S7 8-inch fab shutdown squeezing supply). [official]
  - BMC chips (mature nodes): 11–16 weeks → **21–26 weeks**. [official]
  - PCBs, CPUs: constraints "already evident." [official]
- Global storage inventory (Feb 2026): only **~4 weeks** vs. 8–12 week safety line. [secondary]

### 7.4 Liquid cooling adoption
- **~22% of newly built facilities** using liquid cooling by 2026 (KAD industry overview); cold plate / direct-to-chip ≈ **65% of the liquid-cooling market** in 2026; default design for NVIDIA GB200/GB300. [secondary — KAD](https://www.kad8.com/server/data-center-liquid-cooling-for-ai-workloads-2026/)
- APAC liquid cooling market: **$1.7B (2025) → $2.1B (2026) → $17.5B (2036)**, 23.6% CAGR; direct-to-chip ~55% share in APAC 2026. [secondary — DataNext Research]
- AI datacenter liquid cooling (global): **$6.6B in 2026 → $38.4B by 2033**, 28.7% CAGR (Market Minds Advisory, Feb 2026). [secondary]
- AI datacenter liquid cooling: **$17.83B by 2036** at 16.9% CAGR (Future Market Insights); direct-to-chip ≈ **47% of revenue in 2026**; hyperscale AI datacenters ≈ **55% of deployment demand by 2026**. [secondary]
- Immersion: single-phase at 120–150 MW annual deployment rate in 2026–2027; immersion <2% of global DC cooling (by IT load) in 2025 → 12–18% by 2031–2035 (analyst roadmap). [secondary — Energy Solutions Intelligence]
- Investment signal: Supermicro cites liquid-cooled rack-scale systems (DCBBS) as a competitive differentiator with margin benefit; Foxconn/Quanta GB300 NVL72 racks ship with complete liquid cooling. [secondary]

---

## 8. Conflicting estimates — flagged explicitly

| # | Conflict | Sources |
|---|---|---|
| 1 | **2026 AI infrastructure spend**: $497B (+~56%, IDC AI Infrastructure Tracker July 2026) vs. **$487B** (IDC via Data Center Knowledge, Sept 2026). Likely different tracker vintages; both secondhand. | [secondary] vs [secondary] |
| 2 | **2026 AI server shipment growth**: >20% (TrendForce Oct 2025) → >28% (Jan 2026) → ~28% (Apr 2026) → **nearly 31%** (Aug 2026). Use the latest (Aug 2026) as current. | [official] series |
| 3 | **Total 2026 server shipment growth**: 12.8% (TrendForce Jan 2026) vs. **~13%** (Apr 2026, revised down from ~20% on component constraints) vs. 19.2% (DIGITIMES-adjacent compilation, unverified) vs. 12.8% cited again in invest-bud compilation. The 19.2% figure is an outlier — treat as [unverified]. | [official]/[unverified] |
| 4 | **ASIC vs. GPU in 2026**: TrendForce Oct 2025 says ASIC shipments "expected to surpass those of GPUs" by 2026; TrendForce Jan 2026 says GPUs = **69.7% of AI server shipments**. Possible scope mismatch (AI-chip units vs. AI-server shipments) — do not merge. | [official] vs [official] |
| 5 | **Dell FY2026 Q4 AI figures**: Data Center Knowledge: $9B AI server revenue (+342% YoY), $43B backlog; July-2026 compilation: +39% total sales with AI server revenue "expected to double next year." Different framings of the same quarter — the DCK numbers are more specific and consistent with the FY27 trajectory ($74B guide). | [secondary] vs [secondary] |
| 6 | **AI server market $ value 2026**: IDC AI-infrastructure $487–497B vs. syndicated AI-server estimates $157–224B vs. TrendForce revenue-implied growth. These measure different scopes (full AI infrastructure incl. storage/network vs. servers only vs. AI-server systems). Not directly comparable. | various |
| 7 | **Next Platform's IDC-derived unit math** ($140K avg × 840K units ≠ stated $137.35B total) is internally inconsistent; use IDC's official $87.4B/$27.5B revenue splits and treat unit/ASP derivations as estimates. | [independent] |
| 8 | **Foxconn "40% share"**: company claim ("global AI server market") vs. brokerages ("NVIDIA AI server racks") — different denominators; the latter projects 50% in 2027. | [vendor-reported] vs [secondary] |

---

## 9. Gaps and follow-ups
- No open-source **AI-server-only vendor ranking** (Dell/Supermicro/HPE/Lenovo/Giga Computing/ASUS/Inspur shares of AI servers specifically) found; IDC/TrendForce/DIGITIMES carry this in paywalled trackers. Giga Computing, ASUS, and Inspur 2026 AI-server figures not captured in open sources.
- No clean analyst figure for **8-GPU server unit volumes** specifically; available proxies: AMD GPU units (UBS), IDC GPU-accelerated server revenue/units, NVIDIA rack estimates (brokerages), pricing/residual-value trackers.
- **2027 forecasts**: CSP CapEx >$850B–$1T+ (TrendForce/industry); AI servers ~19% of shipments by 2027 (TrendForce); NVIDIA rack shipments 100–110K (brokerages); TPU racks 105K (brokerages). No full 2027 AI-server revenue forecast found in open sources.
- ServeTheHome carried no 2026 AI-server market-size figure in the sources surveyed; Data Center Dynamics coverage was earnings/analyst-commentary rather than primary market sizing.
- Figures in this draft reflect research conducted **September 22, 2026**; IDC Q3 2026 tracker (expected ~Dec 2026) and Gartner's October 2026 IT-spending update will supersede several numbers above.

---

## §5 Open verification items (consolidated)

All items below are explicitly unresolved as of September 22, 2026 and must not be treated as facts.

### Vendors — Supermicro & Dell
1. **No published list prices** from either vendor for AI servers — all sales are quote-based.
2. **xAI ~$5B GB200-server deal** (Bloomberg/Reuters, Feb 2025 report) — finalization never publicly confirmed in 2026 sources.
3. Several announcement dates flagged **[unverified date]** in the draft (B300 4U/2OU platform, Blackwell Ultra PR, Vera Rubin GTC item, XE9680L re-announcement).
4. **$20B DataVolt/Saudi campus** figure — market commentary only, not a contracted figure.
5. Corporate-level DOJ/SEC exposure and securities class-action outcomes (Supermicro) still open.
6. Dell Q4 FY26 AI revenue cited as **$9.0B and $9.5B** across sources — conflict.
7. **No 2026 CoreWeave deal found for either vendor** — absence of evidence, not evidence of absence.

### Vendors — HPE, Giga Computing, ASUS, MSI, xFusion
8. **No public pricing** for any AI server (quote-based across the industry).
9. "Cray XD675" and ASUS "ESC N8A-E12" **not corroborated as current 2026 SKUs**.
10. MSI customer/revenue detail — not located.
11. Giga Computing named customers beyond RIKEN — not located.
12. **xFusion product launches** — thin coverage in English; needs Chinese-language follow-up. No source confirms xFusion itself is on the BIS Entity List (Huawei is, since 2019).

### Networking
13. **Arista FY2026 guide conflict**: $11.5B (official) vs $12.6B (single secondary outlet, Sep 19, 2026) — flagged unverified.
14. UEC 1.0.3 currency as of July 2026 — community wiki, not verified against ultraethernet.org.
15. No Quantum-3 product evidence found (roadmap appears to skip to Quantum-X1600).
16. Spectrum-XGS exact announcement date approximate.
17. NVIDIA silicon-photonics H2 2026 shipping is a vendor roadmap claim.
18. Transceiver TAM estimates conflict widely across firms.
19. No SKU-level switch shipment volumes exist publicly.

### Market figures
20. **IDC 2026 vintage mismatch**: $487B vs $497B across publications.
21. TrendForce's ASIC-surpassing-GPU claim vs its own 69.7% GPU shipment figure — scope ambiguity.
22. Dell FY26-Q4 framing discrepancies across sources.
23. Foxconn's "40% of global AI server market" — denominator ambiguity.
24. Next Platform unit math internally inconsistent — use with caution.
25. Syndicated market-size estimates ($157–224B) vs IDC/TrendForce scope — not comparable; scope mismatch.
26. **No open-source AI-server-only vendor ranking** (Giga Computing/ASUS/Inspur figures absent — paywalled in IDC/TrendForce/DIGITIMES trackers).
27. No clean 8-GPU unit volume figure found; no full 2027 AI-server revenue forecast found.

---

*Collection metadata: 4 parallel research waves completed September 22, 2026. All claims carry provenance tags; all working drafts retained alongside this file. Read-only web research; nothing sent externally.*
