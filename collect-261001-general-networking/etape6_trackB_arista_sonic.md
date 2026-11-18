# Step 6 — Track B: Arista + SONiC + Cumulus (Data-Center Fabric & Open Networking)

## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 6 (Network & Security), Track B
**Coverage window:** January 1, 2026 → September 22, 2026 (some 2025 context where explicitly marked)
**Scope:** Arista Networks, SONiC (community + commercial distributions), NVIDIA Cumulus Linux, open-networking landscape (UEC/Super Ethernet, Tomahawk 6, white-box), Arista vs NVIDIA Spectrum competitive positioning
**Status:** Research snapshot. Figures are dated snapshots as of September 22, 2026; re-verify against primary sources before use.

### Provenance legend
- **[official]** — vendor's own press release, docs, earnings statement, or regulatory filing.
- **[vendor-reported]** — figure claimed by the vendor (benchmarks, customer counts, adoption) without independent audit.
- **[independent]** — reputable third-party press (Bloomberg, Reuters, CNBC, The Register, Network World, The Next Platform, SiliconANGLE) or independent measurement.
- **[secondary]** — lower-tier press, blogs, analyst writeups, investment sites, community wikis; useful but treat carefully.
- **[unverified]** — single-source, conflicting, or thinly sourced claims; do not treat as fact.

---

## 1. ARISTA NETWORKS

### 1.1 Financial results 2026

**Q2 2026 (reported August 4, 2026) — first $3B quarter**
- Revenue **$3.036 billion**, +12.1% QoQ, **+37.7% YoY** vs Q2 2025 **[official — Arista Q2 2026 earnings release; confirmed by The Next Platform, Sep 20, 2026]**.
- GAAP diluted EPS **$0.95**, non-GAAP diluted EPS **$1.02** (+39.7% YoY from $0.73) **[official]**.
- GAAP operating margin 45.4%, non-GAAP operating margin **49.9%** (vs 44.7%/48.8% in Q2 2025) **[official]**.
- CEO Jayshree Ullal: "As we deliver our first $3 billion quarter in Q2 2026, it is clear that our Arista 2.0 platform strategy is compelling. Customers see networking as the central nervous system for infrastructure from the client to campus to data and AI centers." **[official]**.
- CFO Chantelle Breithaupt (appointed CFO 2025) cited "strong, broad-based growth" **[official]**.
- Full-year 2026 revenue guidance raised to approximately **$12.6 billion**, implying roughly 40% growth **[secondary — Tickeron earnings recap, ~Sep 14, 2026]**.
- Beat Wall Street consensus (revenue consensus ~$2.83B, EPS consensus ~$0.88) **[secondary — Tickeron]**.
- Non-GAAP gross margin slipped to 63.4% on customer mix and higher component costs **[secondary — Tickeron]**.

**Q1 2026 (reported May 2026)**
- Revenue **$2.71 billion**, +35.1% YoY (vs $2.62B expected); non-GAAP EPS **$0.87** vs $0.81 forecast **[secondary — CoinCentral, ~June 9, 2026; ainvest]**. GAAP operating margin 42.7%, non-GAAP 47.8% **[secondary]**.
- Q2 guidance at the time: ~$2.8B **[secondary]**.
- Balance sheet (Mar 31, 2026): $2.79B cash + $9.56B marketable securities; $1.69B operating cash flow in Q1; total liabilities $8.17B, equity $13.49B; low-debt profile **[secondary]**.

**Context (secondary investment-press framing, treat figures as approximate)**
- Zacks "Bull of the Day" (Sep 16, 2026): revenue more than doubled 2022→2025 ($4.38B → $9.00B); GAAP EPS $1.07 → $2.75 over the same period; $13.3B cash and equivalents, $23.7B total assets vs near-zero debt and $8.9B liabilities; five consecutive years of EPS beats; Zacks Rank #1 (Strong Buy) **[secondary — Zacks]**.
- Bank of America raised price target to **$200** (Buy) after the 1.6T launch; average analyst target ~$186.47 **[secondary — CoinCentral]**.
- Major shareholder Andreas Bechtolsheim (co-founder) sold 240,000 shares on June 4, 2026 under a pre-arranged 10b5-1 plan **[secondary — CoinCentral]**.
- AI hyperscalers projected to spend ~$800B in AI-related capex in 2026 **[secondary — Zacks]**.

### 1.2 AI networking revenue and Etherlink portfolio

**AI revenue trajectory (vendor-reported/management statements — handle with care)**
- Management expects AI revenue to reach **at least $3.6B in 2026**, supported by scale-up, scale-out and scale-across deployments **[secondary — Zacks, Sep 16, 2026]**.
- Earlier framing (late 2025 / Feb 2026 reports): Arista **doubled its AI networking revenue goal to $3.25B for FY2026**, up from ~$1.5B in FY2025 **[secondary — Financial Freedom is a Journey, Feb 2026]**.
- ⚠️ The $3.6B figure supersedes the $3.25B figure; use the most recent (Sep 2026) as the current target. Both are management guidance, not independently audited.
- Etherlink AI fabric portfolio: **more than 100 cumulative customers** as of Q2 2026, up from "only a handful of early adopters in 2024" **[secondary — Tickeron Q2 2026 recap; Zacks]**.

**Etherlink AI — platform positioning**
- Arista's AI Ethernet fabric portfolio spans **400G, 800G and (since June 2026) 1.6T** switching **[secondary — The CODEW, Aug 2026]**.
- Technical foundation: RoCEv2 + PFC + DCQCN congestion control, Arista dynamic load balancing (DLB), plus per-packet spraying, congestion signaling, and trimming features aligned with the Ultra Ethernet transport trajectory — described as a "bridge today (RoCEv2 + PFC + DLB) with UEC plumbing being added" **[secondary — LLM-Systems-Wiki Arista/Etherlink page, updated Aug 2026; vendor-reported]**.
- **7700R4 Distributed Etherlink Switch**: single-hop distributed architecture designed to scale to **more than 30,000 connected accelerators** while preserving lossless, deterministic forwarding — an alternative to conventional multi-tier spine-leaf for very large clusters **[secondary — The CODEW]**.
- EOS implements RoCEv2, DCQCN, and priority flow control alongside Arista's own load-balancing for AI workloads **[secondary — The CODEW]**.

**7060XE7 Series — 1.6T launch (June 9, 2026)**
- Announcement: "Arista Introduces Next-Generation 1.6Terabit Portfolio for AI Fabrics" — new portfolio of **1.6T networking platforms for rack-scale AI infrastructure** **[official — Arista press release, June 9, 2026]**.
- Built on **Broadcom Tomahawk 6** silicon; up to **100 Tbps** system switching capacity; 1.6 Tbps per port **[official]**.
- Models (per press release and press coverage):
  - **7060XE7-64PS** and **7060XE7-64PRS**: air-cooled 4U rack switches supporting pluggable IHS (integrated heat sink) and RHS (riding heat sink) optics; IHS for current air-cooled data centers, RHS for future liquid-cooled fabrics **[official]**.
  - **7060XE7-64PRS-RV3-L**: specialized 2OU liquid-cooled platform for high-density clusters, **224G SerDes**, DC power from ORv3 rack, no internal fans, integrates with liquid-cooled XPU servers; availability **Q1 2027** **[official / secondary — Network World]**.
  - **7060XE7-128PE**: **128 ports of 800G** in an air-cooled 4RU design (100G SerDes), for environments needing deployment flexibility and backward compatibility **[official]**.
  - Air-cooled rack switches available **Q4 2026** **[secondary — Network World]**.
- **Linear Pluggable Optics (LPO)**: headline claim of ~**60% reduction in interconnect power consumption** vs traditional optics **[official]**.
- **XPO high-density liquid-cooled pluggable optics**: claimed to reduce networking racks by up to 75% and save up to 44% of floor space vs traditional optics **[secondary — ainvest, citing launch materials]**.
- Runs **Arista EOS** (low-latency, intelligent packet buffering for AI microbursts and collective patterns) and also supports **open network OS options** (SONIC, OpenSwitch) **[official / secondary — Network World, Zacks]**.
- **Hyperscaler endorsements in the launch**:
  - Meta, Microsoft and Oracle contributed statements to the product announcement **[official — Arista press release]**.
  - Microsoft's **Rani Borkar** noted collaboration on the 1.6T Ethernet interface for **Azure Maia**, Microsoft's AI accelerator chip **[secondary — CoinCentral]**.
  - Meta endorsement quote (from press release): "Arista Networks' 1.6T platforms provide the throughput, determinism, and stability needed for our RDMA-based AI fabrics, while Arista EOS delivers operational consistency and performance at scale across our global AI infrastructure." **[official — Arista press release]**.
  - Platform designed to work with **AMD's compute silicon and network interface cards** **[official]**.
- Arista's strategic framing: transition "from providing high-performance switches to delivering comprehensive rack-scale systems" — the network as "an elastic and integrated backplane" / "tightly integrated AI supersystem" rather than a standalone layer **[official — Tyson Lamoreaux, SVP Cloud and AI Networking, Arista]**.

### 1.3 EOS, CloudVision, NetDL, AVA — 2026 software updates

- **EOS (Extensible Operating System)**: single-OS architecture running across Arista's entire hardware line from campus edge to AI spine — longstanding differentiator vs multi-OS vendors **[secondary — The CODEW]**.
- **CloudVision + NetDL**: network-wide management and telemetry layer feeding a **Network Data Lake (NetDL)** that ingests telemetry from Arista hardware, third-party systems, server NICs, and AI job schedulers — explicitly built to correlate network behavior with AI training-job performance **[secondary — The CODEW]**.
- **AVA (AI-driven network automation)**: built on EOS with NetDL as unified data repository; 2026 enhancements announced **December 2025** (GA anticipated Q1 2026):
  - Agentic AI framework: multi-domain event correlation, agentic conversational/troubleshooting with context-aware dialogue, continuous monitoring and automated root cause analysis across wired, wireless, data center and security domains **[secondary — ITDigest, Dec 11, 2025]**.
  - Announced as part of "AI-Powered Campus Mobility at Scale" launch alongside new campus products **[secondary]**.
- **Campus / enterprise (Dec 2025 announcements, GA expected Q1 2026)**: two ruggedized switches (**710HXP-28TXH** and **710HXP-20TNH**) for industrial/outdoor environments (extreme temperatures, vibration, shock; IP50 Din-Rail 20-port and IP30 1RU 24-port with 90W PoE); Wi-Fi 7 access points; all run EOS and integrate with CloudVision **[secondary — ITDigest]**.
- One secondary report targets **$1.25B in campus revenue in 2026**, taking share from Cisco in corporate/branch **[secondary — Financial Freedom is a Journey, Feb 2026 — unverified]**.

### 1.4 Customer concentration and risks (analyst framing)

- Top three hyperscalers account for roughly **55% of net revenue** (alternative report: ~50% from Meta/Microsoft/Amazon/AWS/Alphabet) — concentration is the top cited risk **[secondary — research-ANET GitHub brief, Jul 2026; FFJ]**.
- Margin pressure: as AI clusters grow, buyer leverage forces volume discounts; management guided 2026 operating margins down slightly to ~46% from historic 48%+ highs **[secondary — FFJ]**.
- Supply-chain risk: 1.6T requires cutting-edge optics and cooling; optics hiccups could delay 2026 deployments **[secondary — Tamar Securities]**.
- Cisco competitive resurgence: Cisco data-center segment +43% YoY in Q1 2026 (IDC); campus/enterprise refresh is a grind **[secondary — ainvest]**.
- Customer-concentration example of volatility: a spending pause by a single Cloud Titan (e.g., Microsoft) can create revenue volatility **[secondary — Tamar Securities]**.

---

## 2. SONiC (SOFTWARE FOR OPEN NETWORKING IN THE CLOUD)

### 2.1 Community status and Microsoft's role

- SONiC is a **Linux Foundation project** (moved there in 2022) developing an open-source, Debian-based network operating system for switches, hardware-agnostic and modular **[independent — Network World, Jan 2026]**.
- Started life at **Microsoft**, adapted from its Debian-based Azure Cloud Switch; Microsoft remains a core contributor/user **[independent — The Register, May 27, 2026]**.
- SONiC 4.5 released May 2025, community-supported until at least October 2026 **[independent — Network World]**.
- Commercial vendors (Dell, Cisco, Juniper, Arista, NVIDIA, etc.) support SONiC on their hardware; Arista's 7060XE7 explicitly supports open network OS options (SONIC/OpenSwitch) **[independent/secondary]**.

### 2.2 2026 releases and distributions

**Enterprise SONiC 4.6 (Stordis) — GA June 2026**
- Operations-focused release: route-table **overflow detection with automatic recovery and retry**; improved route consistency checker (per-prefix inspection, transient-route ignore, CLI-triggered recovery); better telemetry; tighter access control; broader hardware list **[secondary — Stordis release breakdown]**.
- Base system moved to **Debian 12 Bookworm**, Linux kernel 6.1; Broadcom SAI Adapter 15.3.0; Broadcom SDK 6.5.35; FRR 8.2.2; Config DB version_4_6_1 **[secondary]**.
- Covers leaf-spine fabrics, Ethernet networks for AI clusters, and campus edge **[secondary]**.
- ⚠️ Release-notes documentation footnote labels Debian 12 as "Bullseye" in one component table; the feature section confirms Bookworm — minor doc inconsistency flagged by Stordis **[secondary]**.

**Netberg SONiC 202511.n0 — released April 15, 2026**
- Successor to the 202411 release; positioned for data centers, AI-driven infrastructure, and campus networks **[secondary — PRLog/Netberg]**.
- Key features: **AI workload optimization** (enhanced **SRv6** support for high-performance AI training/inference backend networks with efficient load balancing); advanced observability (high-frequency telemetry, advanced diagnostics); **Debian 13 (Trixie)** base with updated kernel, security patches, modern toolchain **[secondary]**.

**Aviz Networks turnkey enterprise SONiC — announced December 9, 2025**
- "Aviz Certified Community SONiC": full lifecycle management of community SONiC with OEM-NOS rigor — one continuously qualified code base; **50%+ savings** claimed from software-first operations; 24×7 global support with direct escalation and RMA handling; layered security with SLA-backed CVE patching **[secondary — BusinessWire, Dec 9, 2025]**.
- Certified image qualified across Accton/Edgecore, Celestica, **Cisco, NVIDIA**, Wistron (more coming); FTAS test reports across 100+ enterprise deployment scenarios at the OCP-certified **Aviz ONE Center**; validated runbooks for data center, edge, and **AI fabrics (IP CLOS, EVPN/VXLAN, MLAG, OpenStack, Spectrum-X)** **[secondary]**.
- CEO Vishal Shukla: "Since 2023, SONiC enterprise deployment has shifted from early adopters to large, mainstream enterprises." **[secondary]**.

### 2.3 Major adopter moves 2026

- **Cisco extends SONiC to Nexus 9000** (reported May 27, 2026): Cisco — which supported SONiC on routers for years — announced it will soon extend support to its **N9000 series data-center switches**. Quote: "The N9000 Series is expanding to include a foundation for SONiC, built on Cisco Cloud Scale and Silicon One — alongside platforms powered by NVIDIA Spectrum-X Ethernet switch silicon for AI-class fabrics." Analyst framing: hyperscalers use SONiC for the AI-service networks; as enterprises build on-prem AI (many won't run AI in the cloud), SONiC on everyday data-center hardware "is an idea whose time has come" **[independent — The Register]**.
- **Arista 7060XE7** supports SONiC/OpenSwitch as open-OS alternatives to EOS **[secondary — Zacks, Network World]**.
- **Dell** has long argued SONiC is ideal for enterprise switching and supported it while promoting NOS choice **[independent — The Register]**.
- Dell, Cisco, Juniper, Arista, NVIDIA all support SONiC on their hardware **[independent]**.

### 2.4 Key networking features (SONiC general, per Network World state-of-play)

- MCLAG (L2/L3), weighted ECMP, BGP/OSPF/BFD/IS-IS via FRRouting **[independent]**.

---

## 3. NVIDIA CUMULUS LINUX

### 3.1 NVIDIA stewardship and release status

- Cumulus Networks was acquired by **NVIDIA in 2020**; Cumulus Linux is NVIDIA's switch-optimized Linux-based NOS **[independent — Network World, Jan 2026]**.
- **Latest releases (as of early/mid-2026)**:
  - **Cumulus Linux 5.15** — most recent mainline version (not LTS) **[independent — Network World]**.
  - **Cumulus Linux 5.14** — Debian Bookworm-based **[official — NVIDIA cumulusnetworks/docs GitHub]**.
  - **Cumulus Linux 5.11** — LTS release (debuted 2024), supported **until 2027** **[independent — Network World]**.
  - Cumulus Linux 5.5/5.3 documentation marked "old" (Debian Buster-based) **[official — NVIDIA docs GitHub]**.
- **Support policy [official — NVIDIA docs]**: NVIDIA supports mainline and LTS branches as separate codebases; LTS prioritized for stability (security + critical bug fixes only, no new functionality); mainline gets 12 months of support after the LTS release to migrate. **End-of-life releases are not supported.**
- **NVIDIA's current recommendation [official]**: run the latest **Cumulus Linux 5.y.z** on Spectrum switches; the latest **4.3.z** on Broadcom switches.
- Cumulus Linux 5.5+ includes the **NVIDIA NetQ** agent and CLI for data-center network monitoring and operational health **[official — NVIDIA docs]**.

### 3.2 Cumulus VX / NVIDIA AIR (2025–2026 development)

- NVIDIA **no longer releases Cumulus VX as a standalone image** (per the Cumulus Linux 5.13 "What's New" document): to simulate a Cumulus Linux switch, use **NVIDIA AIR** — the cloud-hosted data-center simulation platform (digital twin of IT infrastructure) **[secondary — ipSpace.net blog, Jun 2025 post; noted updated Mar 2026]**.
- **Update (March 29, 2026)**: a reader report claims NVIDIA has resumed publishing Cumulus VX images, but the newer images are **only accessible with a support contract** — single-reader claim, **[unverified]**.
- Commentary (ipSpace): the forced-NVIDIA-AIR move mirrors past failed vendor attempts (Juniper Junosphere, Cisco before CML); the cloud-only requirement drew community criticism **[secondary — ipSpace.net]**.

### 3.3 Key features and ecosystem

- **Unnumbered interfaces** (simplified BGP/OSPF template for leaf-spine); **Redistribute Neighbor (RDNBR)** for VM/host mobility with L3 discovery; **Prescriptive Topology Manager (PTM)** for connection verification; **NVUE** (NVIDIA User Experience) full-CLI object model enabling advanced programmability **[independent — Network World]**.
- **FireMon** added NVIDIA Cumulus support to its Policy Manager (2025.2.6 feature release, ~May 2026): unified security policy management for open-networking fabrics built on Cumulus, alongside firewalls, AWS/Azure/GCP, Zscaler, Cisco ACI/NSX **[secondary — DataCenterNews Asia]**.
- **NetQ** bundled for telemetry and operational health monitoring **[official]**.

---

## 4. OPEN NETWORKING LANDSCAPE 2026

### 4.1 Ultra Ethernet Consortium (UEC) / Super Ethernet

**Specification status**
- UEC is a **Linux Foundation Joint Development Foundation** project founded **July 19, 2023** by AMD, Arista, Broadcom, Cisco, Eviden/Atos, HPE, Intel, Meta, Microsoft **[secondary — LLM-Systems-Wiki UEC page, updated Aug 26, 2026]**.
- **Specification v1.0**: 562-page document released **June 11, 2025**, defining a full Ethernet-based communication stack (NICs, switches, optics, cables) with the new **Ultra Ethernet Transport (UET)** **[secondary]**.
- Current release: **1.0.3 (July 16, 2026)**; 1.0.2 also released in 2026 **[secondary]**.
- Consortium scale: **100+ member companies, 1,500+ participants** **[secondary — Medium/Teradata Labs, Jun 2026]**.
- UET design goals: per-packet multipathing, lossy-or-lossless operation, fast loss recovery, libfabric compatibility; removes in-order-delivery requirement and lossless-fabric dependency of RoCEv2 **[secondary]**.

**2026 technical priorities** (per Chad Hintz, UEC marketing co-chair / AMD, via Network World):
1. **Programmable Congestion Management (PCM)** — implement congestion-control algorithms in a standard language, portable across UE NICs.
2. **Congestion Signaling (CSIG)** — packets carry high-fidelity congestion information for faster, more accurate transport reactions.
3. **Small-message performance** — UET 1.0's 104-byte headers are fine for 4KB packets (2.5% overhead) but costly for 256-byte transactions; UEC pursuing a reduced-size forwarding header to cut overhead roughly in half for optimized deployments (matters for HPC and local scale-up networks).
4. **In-Network Collectives (INC)** — move AI/HPC reduction operations from hosts into the network **[independent — Network World, ~Jan 2026]**.

**Vendor alignment**
- **Broadcom**: "first UE-compatible Tomahawk" switch with ~250ns latency (claimed); UEC-aligned features (Cognitive Routing 2.0, Global Load Balancing) in Tomahawk 6 **[secondary — Medium; financialcontent]**.
- **Arista**: founding/steering UEC member; Etherlink roadmap tracks UET (per-packet spraying, NSCC/RCCC, trimming, in-network collectives) **[secondary — LLM-Systems-Wiki]**.
- **NVIDIA**: UEC member (~2024) but pushes its proprietary **Spectrum-X** stack; Spectrum-X is the bundled alternative to UEC Ethernet **[secondary]**.
- Ethernet Alliance 2026 focus: higher bandwidth, AI demands, interoperability events, another 200G/lane plugfest **[independent — Network World]**.

### 4.2 Tomahawk 6 — shipments and deployments

- **Broadcom began volume shipments** of the Tomahawk 6 switch series on **March 12, 2026** — first 102.4 Tbps single-chip Ethernet switch; **TSMC 3nm**; 512×200G SerDes channels; ~250ns latency (UE-compatible variant); Cognitive Routing 2.0, Global Load Balancing to attack incast congestion **[secondary — financialcontent/marketminute, Mar 26, 2026; TechPowerUp]**.
- Positioning: unified scale-up and scale-out AI networks; support for 100G/200G SerDes and co-packaged optics (CPO); designed for clusters of **more than one million XPUs**; open specifications for easy integration **[secondary — TechPowerUp]**.
- **Arista and Cisco integrated TH6** into their latest chassis — reporting "flatter" topologies: large AI clusters built with only **two tiers** instead of three or four, cutting optical transceiver count and GPU-to-GPU latency **[secondary — financialcontent]**.
- **Initial deployments scheduled for clusters of 100,000+ XPUs** **[secondary — StorageReview]**.
- **DriveNets** built AI Fabric switches around the Tomahawk 6 ASIC, targeting AI infrastructures with hundreds of thousands of XPUs; systems expected to ship **Q3 2026** — a visible system-level demand signal for TH6 **[secondary — ainvest, Jul 2026]**.
- Broadcom Q1 FY2026 (Mar 2026): total revenue $19.31B (+29.5%); AI infrastructure revenue **$8.4B (+106%)**; networking segment revenue +60% YoY, with Tomahawk 6 "effectively secured Broadcom's technological lead in data center fabrics"; confirmed **OpenAI as sixth custom-silicon (XPU) customer**; Anthropic $21B "Ironwood Racks" order **[secondary — financialcontent, Mar 6, 2026]**.
- ⚠️ "Beat competitors to the 100T era by at least two full quarters" is a secondary outlet's characterization, not independently verified.

### 4.3 White-box / ODM switching

- IDC no longer breaks out ODM Ethernet revenue publicly; **The Next Platform estimates ODMs at $3.56B in Q2 2026, up 2.43× YoY** — its own estimate, **[unverified as to exact figure; directionally consistent with AI buildout]** **[secondary — The Next Platform, Sep 20, 2026]**.
- Open-hardware ecosystem for SONiC deployments: Accton/Edgecore, Celestica, Wistron, Cisco, NVIDIA (Aviz-certified images) **[secondary — Aviz]**.
- SONiC enables NOS choice on white-box fleets; Dell has promoted this model for enterprise **[independent — The Register]**.

---

## 5. ARISTA vs NVIDIA SPECTRUM — COMPETITIVE POSITIONING

### 5.1 Market-share data (IDC, via press)

**Q1 2026 — NVIDIA takes the data-center Ethernet lead**
- **NVIDIA became the #1 data-center Ethernet switch vendor**: ~$2.1B revenue, **+193% YoY**, **21.5% share** — ahead of Arista (20.7%) and Cisco (17.8%) **[secondary — ainvest, citing IDC; remio.ai]**.
- NVIDIA's share climbed from <4% two years earlier to 21.5% in one quarter; the vehicle is **Spectrum-X** **[secondary]**.
- Overall Ethernet switching market: **$15.4B in Q1 2026, +39.8% YoY** **[secondary — remio.ai]**.
- NVIDIA's data-center Ethernet share was ~11% in late 2025 / 11.6% in Q3 2025 **[secondary]**.

**Q2 2026 — the gap widens (IDC via The Next Platform, Sep 20, 2026)**
- Total Ethernet switch revenue grew strongly; **NVIDIA's Ethernet revenue hit $3.86B (2.8×)**, driven by the GenAI systems business and **Spectrum-X attach rates** on DGX nodes and NVL72 rack-scale systems **[secondary — The Next Platform]**.
- Arista: **$2.53B, +37.6%**; Cisco: $5.43B (+36.2% overall, data-center +43% in Q1); HPE (incl. Juniper): $1.15B (+6.1%); Huawei: $1.55B (+28.6%); ODMs est. $3.56B (2.43×) **[secondary — The Next Platform; Q1 HPE/Huawei figures from IDC via ainvest]**.
- Data-center Ethernet in Q2: **$12.3B, +64.5% YoY**, +23% sequentially; ~129.5M ports sold into the data center (analyst estimate); speeds above 200G are overwhelmingly data-center/AI **[secondary — The Next Platform]**.

### 5.2 Architecture comparison: Etherlink vs Spectrum-X

| Dimension | Arista Etherlink | NVIDIA Spectrum-X |
|---|---|---|
| Switch ASIC | Merchant (Tomahawk 5 / Tomahawk 6 / Jericho 3-AI) | In-house Spectrum-4 (+ Spectrum-6 on roadmap) |
| Endpoint / NIC | Third-party NICs (no own NIC) | BlueField-3 SuperNIC / ConnectX; DPUs bundled |
| Fabric stack | RoCEv2 + PFC + DCQCN today; UEC/NSCC/RCCC roadmap | Integrated platform: Spectrum switches + BlueField DPUs + LinkX cables + CUDA stack |
| Congestion control | DCQCN now; UEC NSCC/RCCC later | Closed-loop TCC via SuperNIC |
| Multipath | DLB, packet spraying, MRC/CSIG (EOS) | Per-packet spraying + TCC |
| UEC standing | **Founding/steering member** | Member (~2024), pushes Spectrum-X |
| Optics | LPO/XPO, multi-vendor | Integrated; Spectrum Photonic (TSMC COUPE silicon photonics) roadmap |
| Best-fit | Open multi-vendor AI fabrics; merchant-silicon + UEC trajectory; no NVIDIA networking lock-in | NVIDIA-GPU AI factories; pre-optimized full stack |

**Comparison sources:** LLM-Systems-Wiki Arista/Etherlink page (secondary, vendor-adjacent technical detail) and StorageReview TH6/Spectrum-Photonic comparison (secondary). Treat table cells as secondary-sourced where marked.

### 5.3 Analyst framing of the competitive dynamic

- **The bullish-on-Ethernet case**: by early 2026, Ethernet had "largely caught up to NVIDIA's proprietary InfiniBand in terms of latency and congestion management," capturing **>65% of new AI back-end deployments**; Meta, Microsoft, Amazon, Alphabet validated Ethernet (RoCE) as equivalent performance at lower TCO vs InfiniBand **[secondary — Tamar Securities, Jan 2026]**.
- **The bearish-on-Arista-multiple case**: Arista's stock is priced on the smallest, most contested slice of its revenue (the AI back end), while NVIDIA — which Arista "spent two decades building its identity on" — just became #1 in data-center Ethernet. "Hyperscalers are no longer buying networking as an isolated silo... It is not as good as it looks when the customer stops buying switches and starts buying GPU clusters that happen to include switches." **[secondary — ainvest, Aug 2026]**.
- **Arista's response**: openness and operational consistency — one EOS binary across DC/AI/routing/campus; mix-and-match accelerators and NICs from multiple suppliers; founding UEC member betting Ethernet becomes the default for AI networking, not just scale-out **[secondary — remio.ai; The CODEW]**.
- **NVIDIA's advantage**: vertical integration — GPU + switch chip + switch + SuperNIC + cables + software as one pre-optimized, single-support-number AI factory; IDC's read: integrated GPU-plus-networking co-design is winning AI deals **[secondary — ainvest; The Next Platform]**.
- "AI back-end" market expected to surpass **$15B annually by end of 2026**; trend shifting from inference to large-scale training **[secondary — Tamar Securities]**.

---

## 6. OPEN VERIFICATION ITEMS

1. **NVIDIA Q2 2026 $3.86B figure**: reported by The Next Platform from IDC data (Sep 20, 2026) — likely total Ethernet switching revenue, not directly comparable to the Q1 $2.1B data-center-only figure. Denominator (total vs data-center segment) needs confirmation.
2. **Arista AI revenue targets**: $3.6B (Sep 2026) vs $3.25B (Feb 2026) management guidance — latest supersedes; both vendor-reported.
3. **NVIDIA #1 DC Ethernet share (21.5% Q1 2026)**: sourced to IDC via secondary investment press (ainvest, remio.ai). No primary IDC release was directly fetched; verify against IDC's published tracker before quoting.
4. **ODM $3.56B / 2.43× Q2 2026**: The Next Platform analyst's own estimate (IDC no longer publishes ODM figures). Directional, not official.
5. **Cumulus VX with support contract (Mar 29, 2026)**: single-reader report via ipSpace — unverified.
6. **Tomahawk 6 volume-shipment date (March 12, 2026)**: from marketminute/financialcontent secondary relay; Broadcom's official announcement not directly fetched.
7. **UEC 1.0.3 (July 16, 2026)**: per community wiki citing UEC release notes — not verified against ultraethernet.org directly.
8. **Broadcom "250ns latency" UE Tomahawk claim**: secondary (Medium/Teradata Labs) — vendor-adjacent, treat as marketing until benchmarked.
9. **7060XE7 availability windows** (Q4 2026 air-cooled; Q1 2027 liquid-cooled RV3-L): from official press release and Network World — GA dates can slip; verify before procurement use.
10. **Meta 1.6T RDMA-fabric quote**: from Arista's own press release (official as a quote, but a vendor-authored endorsement).
11. **Ethernet "65% of new AI back-end deployments"**: Tamar Securities feature article (secondary) — no primary source chain.
12. **Cisco SONiC on Nexus 9000 timing**: "soon" per The Register (May 27, 2026) — GA date not confirmed as of Sep 22, 2026.
13. **Arista campus $1.25B 2026 revenue target**: single secondary source (FFJ) — unverified.
14. **Arista customer-concentration figures (55% top-three)**: secondary analyst brief — approximately right directionally, but exact mix is not disclosed by Arista.

## 7. COLLECTION METADATA

- **Research date:** September 22, 2026.
- **Track:** Step 6 Track B — Arista + SONiC + Cumulus (data-center fabric & open networking).
- **Source priority used:** vendor press releases/earnings docs (Arista Q2 2026, Arista 1.6T press release, NVIDIA Cumulus docs); The Register; Network World; The Next Platform (IDC-derived figures); Stordis/Netberg/Aviz distribution release notes; secondary investment press (Zacks, ainvest, CoinCentral, Tickeron) for earnings color and analyst framing — marked [secondary] throughout.
- **Not fetched directly:** primary IDC Ethernet Switch Tracker release; ultraethernet.org spec history page; Broadcom's official Tomahawk 6 announcement; Arista EOS release notes for 2026 builds; community SONiC GitHub release tags for 2026. These are marked [unverified] or [secondary] where cited via relays.
- **Related prior coverage (same project):** Step 5 Track D (servers) covered 800G adoption, Arista Q2 $3.036B, Spectrum-6/Silicon One G300/Tomahawk 6, Cisco $9.3B hyperscaler AI orders, UEC spec status at high level. This track is the authoritative one for Arista/SONiC/Cumulus detail.
- **Nothing sent externally; read-only web research + local file write.**

---

## 8. SUPPLEMENTARY RESEARCH (independent second pass, 2026-09-22)

Additive findings from a second research pass, all provenance-tagged. Nothing above was removed or altered.

### 8.1 Microsoft Fairwater on SONiC (OCP session, ~June 2026)

- An OCP session featuring **Mehak Mahajan (Broadcom)** and **Guohan Lu (Microsoft Azure)** described **Microsoft's Fairwater AI data center as 500,000 GPUs in one data center — "the world's largest AI infrastructure" — running on SONiC** `[vendor-reported]` (YouTube/OCP session, crawled ~June 2026).
- Fairwater fabric design points from the session:
  - **Multi-plane rail topology** connecting 500K+ GPUs, built with **51.2T ASICs** (Tomahawk 5 class).
  - **BGP scaled to 512 sessions per pizza-box switch** in SONiC (FRR upgrades, sub-second convergence).
  - **SRv6 source routing** — giving the NIC control over traffic spreading; deterministic multipathing to avoid ECMP hash collisions (a companion session, "AI Backend: Deploying SRv6 uSID and SONiC for Deterministic Load Balancing," deep-dives Microsoft's production SRv6 uSID deployment in SONiC).
  - **Packet trimming on Tomahawk 5** — recovering from drops before RDMA times out.
  - **High-frequency streaming telemetry at millisecond cadence** via IPFIX + OpenTelemetry.
  - All four technologies (BGP scaling, SRv6, packet trimming, high-frequency telemetry) are **live in SONiC 2025.11 and running in production at Fairwater** `[vendor-reported]`.
  - Next horizon named in the session: **Ethernet for Scale-Up Networking (ESN)** and the **SONiC Scale-Up Working Group** `[vendor-reported]`.
- ⚠️ "World's largest" and the 500,000-GPU figure are session claims by Microsoft/Broadcom speakers; no independent audit `[vendor-reported]`. Session URLs: https://www.youtube.com/watch?v=1SpuQtOBmjE and https://www.youtube.com/watch?v=yB-_4lkSsEs

### 8.2 Community SONiC 202605 release

- **SONiC 202605** (May 2026): **Debian 13 (Trixie)**, SONiC kernel **6.12.41**, **SAI 1.18.1**, **FRR 10.5.4**, **Redis 8.0.2**, **Docker 28.2.1**, **Python 3.13.5** `[secondary]` (btw.media analysis).
- New/evaluated items classified **Alpha** in the release: OpenConfig YANG dial-out telemetry, limited multi-ASIC warm reboot, BMC Redfish workflows, self-encrypting-drive password operations, telemetry VRF binding, event/alarm framework — release inclusion is not a promise of stable behavior across all platforms `[secondary]`.
- Analyst note from the same source: **"supports SONiC" is too broad for procurement** — a meaningful platform statement identifies hardware, ASIC, image provider, SONiC release, SAI/SDK, tested features, and support owner `[secondary]`.

### 8.3 Nokia–Microsoft SONiC collaboration (context, deal from Nov 2024)

- Nokia expanded its multi-year agreement to supply **Microsoft Azure** datacenter networks with a **5-year deal** (announced Nov 2024): **Nokia 7250 IXR-10e** platform for multi-terabit-scale interconnect plus a custom management top-of-rack switch; **SONiC-based Nokia data-center routers and switches** deployed in greenfield sites and in Microsoft's **100GE→400GE migration**; Nokia's global footprint extended to **30+ countries** `[official]` (Nokia/Nasdaq press release).
- David Maltz (Technical Fellow/CVP, Microsoft Azure Networking): "Over the past six years we have worked with Nokia's engineers to develop their routers running SONiC" `[official]`. The collaboration extended to **chassis-based SONiC support for high-capacity datacenter roles** `[secondary]`.

### 8.4 Cumulus Linux release-train update (NVIDIA official docs, crawled Sept 2026)

| Version | EOL |
|---|---|
| 5.12.z | February 2026 |
| 5.13.z | May 2026 |
| 5.14.z | July 2026 |
| 5.15.z | November 2026 |
| 5.16.z | April 2027 |
| 5.17.z | July 2027 |
| 5.18.z | August 2027 |
| 5.9.z (LTS/ESR) | April 2027 |
| 5.11.z (LTS) | November 2027 |

- **Cumulus Linux 5.y.z supports Spectrum-based switches only**; **4.3.z (Broadcom) is in maintenance mode with no new features planned, EOL December 2025** `[official]`.
- **5.13**: new platform **NVIDIA SN5600D** (800G Spectrum-4 DC version) `[official]`.
- **5.15.0**: NVUE command updates to **standardize commands across Cumulus Linux, NVIDIA OS (NVOS), and host-based networking** — NVIDIA recommends customers review NVUE command changes before upgrading `[official]`.

### 8.5 Arista Q2 2026 — additional color (remio.ai analysis, secondary)

- Etherlink: **>100 customers** vs "four or five" in 2024 — broadening beyond hyperscalers to specialty AI clouds, enterprises, and custom-accelerator operators `[vendor-reported]` (management via remio.ai).
- **Scale-across** target: **~$1.2B in 2026**; management estimates the scale-across market could reach **$15B–$20B by 2030** `[vendor-reported]`.
- **Microsoft and Meta have each represented at least 10% of annual revenue** in recent reporting periods `[secondary]`.
- Arista management acknowledged **meaningful scale-up revenue is unlikely before 2027** (NVLink territory) `[secondary]`.
- Purchase commitments **~$9.7B** at Q2-end (vs ~$3.6B a year earlier); inventory ~$2.5B; deferred revenue ~$6.9B; supply pressures expected **into 2028** `[secondary]`.
- 7060XE7 air-cooled systems: Q4 2026; liquid-cooled + additional configurations: early 2027 `[official]` (Arista press release, cited in §1.2 above).
- Source: https://www.remio.ai/post/arista-networks-outlook-jumps-as-ai-demand-meets-an-nvidia-challenge
