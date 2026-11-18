# Step 6 Track A — Cisco + Juniper (Enterprise Data-Center Networking), 2026

*Research as of September 22, 2026. Read-only web research; no live-browser navigation. English.

---

# PART 1 — CISCO

# Cisco Enterprise/Data-Center Networking 2026 — Research Report

Coverage as of: September 22, 2026.
Prepared by subagent research, read-only web research. No live browser navigation performed.

**Legend:** [official] = Cisco press release, blog, data sheet, earnings release, investor deck | [vendor-reported] = partner (DDN, NetApp, WWT, CDW, Computacenter) quotes in Cisco PR | [independent] = trade press / analyst outlets | [secondary] = aggregators, blogs, opinion pieces, investor blogs | [unverified] = single-source or low-reliability claims; treat with caution.

---

## 1. Executive Summary

2026 was Cisco's breakout AI-networking year, centered on the February 10, 2026 launch of the **Silicon One G300 (102.4 Tbps)** chip at Cisco Live EMEA in Amsterdam, followed by AgenticOps software (Cisco Cloud Control) at Cisco Live Las Vegas (June 2–4, 2026) and a September 15, 2026 Splunk AI POD announcement at Splunk.conf in Denver. Financially, Cisco reported **$9.3B in FY2026 AI infrastructure orders from hyperscalers (4.5× FY2025)** and **~$4B in related revenue** [official via multiple trade press; vendor-leaning but earnings-sourced], guiding to **$7.5B AI infra revenue in FY2027** [independent via Barron's/Trefis coverage of earnings call]. Roughly 60% of AI orders were Silicon One systems, 40% optics [secondary via Trefis]. M&A in 2026 skewed toward AI security/observability (Galileo, Astrix Security, Teleport partnership, Astrix ~$400M [unverified]) rather than data-center switching silicon. No 204.8T chip announcement from Cisco was found in 2026; the top announced density remains 102.4T (G300).

---

## 2. Silicon One — G300 and roadmap

### Silicon One G300 (flagship 2026 announcement)
- **Announced February 10, 2026** at Cisco Live EMEA, Amsterdam [official] (press release via stockhouse/PR Newswire mirrors; original at Cisco newsroom).
- **102.4 Tbps switching capacity**, single-chip; designed for massive "scale-out" AI clusters [official].
- Powers new **Nexus 9000 and Cisco 8000** fixed and modular Ethernet systems, in air- and **100% liquid-cooled** configurations [official].
- Specs per Cisco PR coverage: deterministic, low-latency, power-efficient; supports **64× 1.6T (1600 GbE)** port configurations [independent via eWeek].
- **"Industry's largest on-chip buffer"** claim [vendor-reported] — per quotes in the Cisco PR from WWT ("102.4 terabits of capacity with the industry's largest on-chip buffer"). Exact buffer size in MB/GB was **not disclosed** in the materials found — flagged as a gap.
- G300 marketing framed around "Intelligent Collective Networking": shared packet buffers, proactive telemetry, path-based load balancing to boost GPU utilization and job completion times [independent via SDxCentral/HostNoc summarizing Cisco PR]. These are **marketing terms**; exact mechanisms (beyond Intelligent Packet Flow, ECN, PFC, DLB — see §6) were not technically specified in found sources — flag as uncertainty.
- Partners validating the stack in the PR: **DDN (Sven Oehme, CTO), NetApp (Syam Nair, CPO), WWT (Neil Anderson), CDW (Brian Campbell), Computacenter (Thomas Berger)** [vendor-reported].
- New systems announced alongside: **N9364F-SG3** (G300-powered Nexus 9000, 64 ports 800G/1.6T, 102.4T total) and **N9364E-SP2R-X** (P200-powered, 64 ports 800G/1.6T, 51.2T total) [vendor-reported via CDW quote in PR; treat SKU list as vendor-reported].
- Liquid-cooled G300 systems claim "nearly 70% energy efficiency improvement," delivering in a single system the bandwidth that previously required 6 prior-generation systems [official]. Baseline of comparison not specified — flagged as an apples-to-oranges caveat.

### P200 / G200 (prior generation, context)
- Silicon One **P200** (51.2T) powered 51.2T Nexus 9000 systems announced for hyperscale deployments in 2025; at Cisco Live EMEA 2026 Cisco announced an **expanded portfolio of P200-powered systems** for spine, core, and interconnect roles (neoclouds, SPs, enterprises), plus expanded OS support on 8223 systems [independent via CRN/SDxCentral].
- Five new hyperscaler design wins reported in Q3 FY2026 (per Lightwave) included "systems powered by Silicon One P200 and G200 chips" [secondary]. Note: G200 details not covered in this research pass; **no G500 announcement found as of Sept 2026** — the task's "G300/G500 (or newer)" premise: only G300 is confirmed [independent].

### Port-density announcements (2026)
- 51.2T, 102.4T: confirmed (P200, G300) [official/independent].
- **204.8T: no Cisco announcement found in 2026 research** [independent]. Flagged: if 204.8T-class news exists, it comes from competitors (e.g., Broadcom Tomahawk-class roadmaps), not Cisco's public announcements in the covered sources.

---

## 3. Nexus 9000 new SKUs (2026)

### Nexus 9364F-SG3 (G300)
- G300-powered fixed system; 64 ports of 800G/1.6T; 102.4T total throughput [vendor-reported]. Positioned for hyperscale/enterprise AI scale-out [official/independent].

### Nexus 9364E-SP2R-X (P200)
- P200-powered; 64 ports 800G/1.6T; 51.2T total throughput [vendor-reported]. Announced at Cisco Live EMEA 2026 [independent via CRN].

### Nexus 9332D-H2R (deep-buffer 400G)
- **32-port 400G QSFP-DD**, 1RU, 25.6 Tbps of bandwidth [official via Cisco data sheet].
- **First switch in the Nexus 9000 fixed-switch portfolio with deep-buffer capability**: 80 MB on-die packet buffer + **8 GB high-bandwidth memory (HBM)** [official].
- MACsec on all 32 ports; PTP Class C timing accuracy [official].
- Power: 2000W AC/DC/HV PSUs; typical 1015 W, maximum 1900 W; operating temp 0–40°C [official].
- Cisco positioning: for AI/ML fabrics, with "the hardware and software capabilities to provide the right latency, congestion-management mechanisms, and telemetry to meet the requirements" of AI workloads [official via product page text].
- **Price (third-party listing)**: Zones listed N9K-C9332D-H2R at **$128,731.99** [secondary]. List price from Cisco not found — treat as retailer pricing, may vary.
- NOTE: The data sheet found is ©2026; the product was introduced earlier (the SKU predates 2026 per some © dates) — flag that it is not strictly a "2026 launch" but is Cisco's current deep-buffer 400G fixed option and featured in 2026 AI-cluster white papers.

### Other SKUs referenced in 2026 materials
- **N9364E-SG2-Q** (Nexus 9300, 64× 800G QSFP-DD spine-leaf) — appears in the 2026 Cisco AI POD design for NVIDIA 2-8-9-400 Enterprise Reference Architecture (6 units in backend fabric) [official via Cisco CVD PDF].
- **N9K-C9364D-GX2A** (64-port 400GE) — used as frontend/spine-leaf in AI POD BOM and in Cisco's own 256-GPU (H100) and 512-GPU (H200) reference cluster designs [official via Cisco white paper/CVD].
- **Nexus 9364E-SG2** (high-density 800G aggregation, 400/200/100G breakout support) [independent via Network World, referencing Cisco blog].
- Cisco used a **rail-optimized, non-blocking backend network** for its 256× NVIDIA H100 GPU cluster (24× Nexus 9364D-GX2A per 512× H200 design) [official via Cisco AI-clusters white paper].

---

## 4. Cisco AI PODs — 2026 updates

### Program overview (2026 data sheet)
Cisco AI PODs = pre-validated, modular, full-stack AI infrastructure under **Cisco Secure AI Factory with NVIDIA** [official]. 2026 data sheet specs:
- GPU density: 32, 64, 128+ scale units; CPUs: dual Intel Xeon Scalable or AMD EPYC; up to 4 TB DDR5/node; NVMe up to 30.7 TB/node [official].
- **Networking: 400G/800G Nexus 9000 series, RoCEv2, lossless, sub-ms latency** [official].
- GPU options: NVIDIA **H100, H200, RTX Pro 6000, L40S, A100**, plus AMD **MI300X** [official].
- Storage: VAST Data, NetApp AFF, Pure Storage FlashArray, Nutanix [official].
- Management: Cisco Intersight + Nexus Dashboard; OS: RHEL, Ubuntu, Rancher, Red Hat OpenShift; security: Hypershield, AI Defense, Isovalent, SSO/AAA [official].
- Ordering SKUs: **AIPOD-POD1** (inferencing/edge), **AIPOD-POD2** (training/fine-tuning) [official].

### 2026 new/updated POD items
1. **Cisco AI POD for Splunk** (Sept 15, 2026, announced at Splunk.conf, Denver) — newest POD configuration within Cisco Secure AI Factory with NVIDIA. Pre-validated stack: Cisco infrastructure + NVIDIA accelerated computing + new AI runtime + Kubernetes, optimized for Splunk AI workloads; targets self-managed on-prem, private cloud, and air-gapped environments. Splunk AI Assistant available now; Agent Launchpad later in 2026; self-hosted models: Cisco Deep Time Series Model, Google Gemma 4, OpenAI GPT-OSS 20B; NVIDIA Nemotron planned. Day-one delivery partners: **Accenture, bitsIO, Wipro, WWT** [secondary via AI Market Watch and eeNews Europe; flagged secondary because Cisco PR not directly found in this pass].
2. **Cisco AI POD Infrastructure for NVIDIA 2-8-9-400 Enterprise Reference Architecture** (2026 CVD) — UCS C885A M8 servers with 8× NVIDIA H200 SXM GPUs, BlueField-3 SuperNICs, Nexus N9364E-SG2-Q backend + N9K-C9364D-GX2A frontend fabrics [official via Cisco design guide].
3. **Internet2 / National Research Platform deployment** (announced ~Sept 21, 2026 via PR Newswire): two AI PODs at two campuses (University of Maryland? — campuses named in full PR; snippet names them via Internet2; specific campus names not captured — flag as gap) adding GPU capacity to the NRP's Nautilus Kubernetes fabric. Combined: **8× NVIDIA H200 GPUs (1.1 TB combined GPU memory), 128 CPU cores, 2 TB system memory, 90 TB NVMe**; each site one UCS C845A M8 chassis with 4× H200 on NVLink [official via PR Newswire release].
4. **Secure AI Factory with NVIDIA** context (announced March 2025, i.e., pre-2026 but foundational): hyperscale-class AI cluster architecture from Cisco + NVIDIA; Nexus One management plane spanning NX-OS and SONiC; Nexus Dashboard AI job monitoring [independent/secondary].

### Customer deployments found
- **Luma AI** (generative video startup): anchor customer for the AMD/Cisco/HUMAIN JV's first 100 MW Saudi deployment [independent via Reuters-cited reports — see §8].
- **Internet2/NRP**: two university campuses (above).
- Hyperscalers: named hyperscaler wins are **not publicly disclosed** by Cisco (orders reported in aggregate). Flag as a gap: no named hyperscaler Cisco win found in sources; the "large customer wins" story is mostly aggregate-order figures, not logos.

---

## 5. Financials — AI infrastructure orders/revenue (earnings calls)

Cisco's fiscal year ends late July (FY2026 ended July 25, 2026). All figures below from earnings coverage [independent, multiple outlets consistent]; exact Cisco IR press-release text not directly verified in this pass — flag as independently-reported-but-earnings-sourced.

### Q3 FY2026 (reported May 13, 2026)
- Revenue: **$15.84B** (+12% YoY, record), adjusted EPS $1.06 [independent via The Weighted Average].
- **$1.9B in hyperscaler AI infrastructure orders in Q3** (triple YoY); **$5.3B booked YTD**; raised FY2026 AI-orders target to **~$9B** (4.5× FY2025), from prior $5B [independent via Barron's/biggo].
- **Raised FY2026 AI infrastructure revenue guidance to $4B** (from $3B) [independent].
- Segment color: total networking product orders +50%+ YoY; data-center switching orders +40%+; campus networking +25%+ [independent via The Weighted Average].
- Same release announced **~4,000 job cuts** over the following three quarters (restructuring alongside AI growth) [independent].
- Q3 also included: Acacia record quarter — **750,000+ 400G and 40,000 800G coherent pluggable optics shipped**; five new hyperscaler design wins; Acacia projected to **grow 200% in FY2026** per CEO [independent via Lightwave].
- Zacks analyst note (May 2026): Cisco expected **>$3B AI infra revenue from hyperscalers in FY2026**; pipeline of **>$2.5B orders** from sovereign/neocloud/enterprise; Q2 FY2026 took **$350M** from neocloud/sovereign/enterprise (separate from $2.1B hyperscaler orders in that quarter) [secondary; investor-relations deck corroborates the $350M Q2 non-hyperscaler AI orders figure — official via Cisco IR deck].

### Q4 FY2026 (reported Aug 12, 2026)
- Revenue: **$17.3B** (+18% YoY, record); non-GAAP EPS **$1.22** (+23%); beat consensus ($16.8B / $1.17) [independent via Barron's].
- **AI infrastructure orders from hyperscalers: $4B in Q4**; **$9.3B for FY2026** (4.5× FY2025; above the $9B target) [independent, consistently reported].
- **AI infrastructure revenue (hyperscalers): ~$4B in FY2026** [independent].
- Networking revenue: **$9.79B in Q4** (+28% YoY) [independent]. Product orders +35% YoY; hyperscaler orders triple digits; networking product orders +40% (8th consecutive quarter of double-digit growth); data-center networking orders +~25% YoY for FY2026 [independent].
- **FY2027 guidance**: total revenue **$72.2–73.4B** (midpoint ~15% growth off $63.3B FY2026 revenue); non-GAAP EPS **$5.05–5.11**; non-GAAP operating margin ~35%; **AI infrastructure revenue from hyperscalers guided to $7.5B** [independent]. 4–5 percentage points of FY2027 revenue growth expected from price increases; memory costs pressuring gross margin [independent via Trefis].
- Margin/headwind caveats: gross margin pressured by higher hardware mix and memory costs; ~60% of AI orders are Silicon One systems, 40% optics [secondary via Trefis]; CEO argues the orders are still highly profitable (low incremental sales expense) [secondary].
- **Nexus switch orders tagged for AI deployments: up >85% sequentially** (CEO Chuck Robbins, Q4 call) [secondary via Benzinga/cloudfront mirror — treat as secondary since not from a transcript).
- Market reaction: stock fell after-hours despite beat [independent via Barron's/Blockonomi].

### Trailing history (context)
- Q1 FY2026: **$1.3B** hyperscaler AI infra orders + **>$200M** from neocloud/sovereign/enterprise [secondary via Motley Fool].
- "Cisco raised AI infrastructure revenue guidance in FY2026 from $2B to $3B" (earlier in year per Network World) — superseded by $4B actual [independent].

---

## 6. Key 2026 product launches: Nexus Dashboard, fabric, AI-cluster networking

### Nexus One (launched at Cisco Live EMEA, Feb 2026)
- New management platform creating a **unified fabric** from NX-OS VXLAN EVPN and Cisco ACI fabrics (open standards), with consistent experience via Nexus Dashboard [independent via CRN].
- Features: unified fabric (deploy fast, adapt as demands shift, built-in automation); **AI job observability** built in; **native Splunk integration** (from March 2026) — analyze network telemetry where it resides without moving it, positioned as "essential" for sovereign clouds [independent via CRN].
- Per secondary coverage: Nexus One provides a unified management plane across **NX-OS and SONiC** environments, with on-prem Nexus Dashboard and cloud-managed Nexus Hyperfabric as operational models; **Nexus Dashboard 4.2** adds deep AI job-monitoring, telemetry-based congestion control, intelligent auto-adjusting load balancing, real-time GPU health tracking [secondary via National Cyber Security Consulting — flagged secondary, not directly verified against Cisco docs].

### Cisco Cloud Control + AgenticOps (Cisco Live, Las Vegas, June 2, 2026)
- **Cisco Cloud Control** (controlled availability in the US from June 2, 2026; global availability planned July 2026): unified platform across networking, security, observability, collaboration; foundation of the "AgenticOps" model [independent via CRN/Presidio].
- **AI Canvas**: multiplayer workspace where operators + AI agents work from shared live telemetry; context persists across shifts [independent].
- **Agent Builder / App Builder / Cloud Control Marketplace** (integrations: ServiceNow, Atlassian, BMC, Okta, Ping, Entra ID, Jamf, LiveAction, Panduit, NetBox, Device42, Vertiv, Anthropic, OpenAI, NVIDIA, Collibra) [independent via GIXtools].
- **Agentic Actions for networking** (beta June 2026 via Meraki): sense → diagnose → remediate → validate → deploy loop; Experience Metrics; Deep Reasoning; **Digital Twin** (alpha July 2026) — emulated network replica using actual software images [independent via GIXtools].
- **Cisco Multicloud Fabric**: cloud-delivered service connecting branches/DCs/clouds (AWS, Azure, GCP, neoclouds), managed overlay, no customer hardware, zero-trust routing, ThousandEyes + Splunk observability [independent].

### Congestion-control / AI-cluster networking features (Nexus 9000)
Cisco's AI/ML fabric feature set (from Cisco blog + white papers, [official]):
- **Dynamic Load Balancing (DLB)**: distribute traffic across equal-cost paths/links.
- **Priority Flow Control (PFC)**: pause frames to prevent Ethernet frame drops → lossless Ethernet for RoCEv2.
- **Explicit Congestion Notification (ECN)**: end-to-end per-IP-flow congestion marking without drops.
- **WRED** (Weighted Random Early Detection) for traffic-class drop probabilities.
- **Cisco Intelligent Packet Flow**: proactive congestion management for non-uniform inter-switch link utilization, "tailored to customer-specific requirements" [official via Cisco AI-networking white paper].
- Lossless transport for **RoCEv2** with Data Center Bridging (DCB); sub-microsecond latency on 400G/800G interfaces [official/independent].
- DCN licensing note: fine-grain load balancing on 800G switches sold as perpetual license **DCN-FGLB-XF3** (seen in AI POD BOM) [official via CVD].
- PFC/ECN are also the technologies the **Ultra Ethernet Consortium** is developing for AI infrastructure — Cisco positioning aligns [independent via Network World].
- **Nexus Dashboard** as ops/automation platform: Insights (visibility), Fabric Controller (automation) [official].

### Nexus Hyperfabric AI
- Cloud-managed fabric with intent-based networking: cloud design tools, zero-touch deployment, declarative policy, automatic GPU-cluster detection; workload-aware observability (network behavior correlated with AI job performance), ML-powered root-cause [secondary via Medium author's article — flagged secondary; Cisco's official Hyperfabric branding exists but details above are from a secondary source].

---

## 7. 2026 acquisitions and partnerships (networking/AI infrastructure relevance)

### Acquisitions
1. **Galileo Technologies Inc.** — AI observability/"AI trust" platform; announced ~April 2026 (Cisco's "second planned deal of 2026"); terms **undisclosed**; expected close in **Q4 FY2026**; strengthens Splunk Observability Cloud AI-agent monitoring [secondary via CRN/TipRanks]. Network World (April 2026) described Galileo as "purposeful observability for AI agents across the full AI development lifecycle" [independent].
2. **Astrix Security** — Israeli startup (founded 2021; non-human identity / AI-agent security: discovers AI agents, maps non-human identities — API keys, service accounts, OAuth tokens, MCP servers; lifecycle management, automated remediation). Timeline: "advanced talks" reported April 14, 2026 at **$250–350M** [secondary via The Information/CRN]; Cisco announced **intent to acquire May 4, 2026** [independent via SecurityWeek mirror (obstracts)]; **~$400M** per Calcalist/CTech [secondary, flagged as press-reported not official]. Terms officially undisclosed. Astrix had raised $85M total ($45M Series B Dec 2024, Menlo Ventures-led) [secondary]. Planned integration into identity intelligence, secure access, Duo IAM [independent].
3. **NeuralFabric Corp.** — enterprise AI platform (generative AI, data sovereignty); announced end of 2025 (context, just outside 2026) [secondary via CRN].
4. **EzDubs** — real-time speech translation; end of 2025 [secondary via CRN].

### Partnerships
1. **AMD + Cisco + HUMAIN joint venture** (announced Nov 19, 2025; operational ramp 2026) — up to **1 GW of AI infrastructure in Saudi Arabia by 2030**; initial **100 MW** phase with AMD Instinct MI450-series GPUs + Cisco networking/critical infrastructure; HUMAIN (PIF-backed) provides data centers. AMD and Cisco are **exclusive technology partners and founding investors**. First customer: **Luma AI** contracted the full 100 MW. By **Aug 31, 2026**, the first segment became operational running **AMD MI355X GPUs**; up to 250 MW more planned from 2027 [independent via Data Centre Magazine/StorageReview/CryptoBriefing]. Note: announced Nov 2025, so 2026 status = first phase operational.
2. **Cisco + Teleport** (announced Aug 25, 2026) — strategic partnership on "Infrastructure Identity" (short-lived, task-scoped access for humans and AI agents); Cisco became Teleport's **largest strategic investor**; first use case: privileged access for network/infrastructure admins [secondary via Cyber Magazine]. Security-flavored, not DC networking hardware.
3. **Cisco joins Verizon's 6G Innovation Forum** (Sept 17, 2026) as core partner — AI-native networking research for enterprise/carrier infrastructure [secondary via Simply Wall St].
4. **Splunk + AWS** multi-year joint-development pact for agentic SOC products (announced ~Sept 2026 alongside Splunk.conf) [secondary].
5. AI POD ecosystem partners (ongoing 2026): NVIDIA, VAST Data, NetApp, Pure Storage, Nutanix, WWT, CDW, Accenture, bitsIO, Wipro, Computacenter [official/vendor-reported].

---

## 8. 400G/800G portfolio, optics, CPO

### Pluggable optics launched Feb 2026 (with G300)
- **1.6T OSFP** pluggable optics: ultra-high-bandwidth for AI scale-out; targets 1.6T switch-to-NIC links and 1.6T/800G/400G/200G switch-to-server links; high performance + reliability [official]. Both ship **this calendar year** per Cisco (CRN) [independent].
- **800G Linear Pluggable Optics (LPO)**: **50% lower optical-module power vs. retimed modules**; with LPO-supporting N9000/8000 systems, customers can cut **overall switch power by 30%** [official]. Built with Cisco silicon photonics technology [secondary via Zacks].
- AI POD 2-8-9-400 BOM shows: QSFP-400G-DR4 (400G QSFP112), QDD-8X100G-FR dual-port 800G, QSFP-200G-SR4-S in use [official via CVD].

### Optical transport (DCI/backbone, context)
- **Cisco Open Transport 3000 Series** multi-rail open line system (2026): Cisco claims 75% power reduction and 80% rack-space reduction per rail vs. prior single-rail systems (C-band and C&L-band) [independent via Light Reading]. Primarily hyperscalers/SPs; "14× capacity" framing vs. legacy DCI [independent].
- **NCS 1014** upgraded with 800G pluggable transponder line card: 12.8T capacity (16× 800G in one card), 50% less rack space, 38% less power vs. prior 400G generation [independent via Light Reading].
- **New 100ZR coherent pluggable** using Cisco's own silicon photonics + DSP; 100–150 km reach; extends Routed Optical Networking into access/edge [independent via Light Reading].
- Acacia (Cisco's coherent unit): record Q3 FY2026 — **>750,000 400G + 40,000 800G coherent pluggables shipped**; five new hyperscaler design wins (incl. Silicon One P200/G200 systems); CEO: Acacia to **grow 200% in FY2026** [independent via Lightwave]. "Commanding share" of the ZR/ZR+ market [independent].

### Co-packaged optics (CPO)
- **No Cisco CPO product announcement found in 2026 research.** Cisco's 2026 posture is pro-**pluggable**: 1.6T OSFP + 800G LPO (headlined "Cisco bets big on pluggables" [independent via Light Reading]). Cisco's CPO work appears limited to groundwork ("laying the groundwork for solutions that will enable seamless and scalable AI at unprecedented densities" [secondary via JR Sekwele]). CPO commercial debut in 2026 is industry-wide (NVIDIA Quantum-X/Spectrum-X Photonics cited as leading; Broadcom Bailly) [secondary]. Cisco's near-term efficiency play = **LPO**, not CPO. Flag as a competitive gap vs. NVIDIA/Broadcom CPO roadmaps.

### 800G/400G switch portfolio status
- 400G: Nexus 9364D-GX2A (64p 400G), 9332D-H2R (32p deep-buffer 400G, HBM), 9364D-GX2A-class 25.6T boxes; breakouts down to 10G [official].
- 800G: N9364E-SG2-Q (64× 800G QSFP-DD), N9364E-SP2R-X (P200, 51.2T), N9364F-SG3 (G300, 102.4T), 800G fixed (XF3-class DCN licensing) [official/vendor-reported/independent].

---

## 9. Cisco vs. competitors — AI networking positioning (2026)

- **Market position**: Cisco remains the overall Ethernet switch/router market leader but **lost early AI-cluster share to Arista and NVIDIA** in the hyperscaler buildout [secondary via Motley Fool]. 2026 narrative: Cisco "finally winning over hyperscalers" — $9.3B FY2026 hyperscaler AI orders as evidence [secondary].
- **Cisco's claimed differentiation**: (a) enterprise + SP + hyperscaler breadth vs. Arista's hyperscaler focus; (b) multi-vendor interoperability vs. NVIDIA Spectrum-X's stack lock-in; (c) lifecycle support, integrated security (Hypershield/AI Defense), cross-domain policy; (d) largest on-chip buffers, Intelligent Packet Flow [vendor-reported/independent].
- **Named competitive displacements**: Q4 FY2026 earnings coverage notes Cisco said it is **"taking customers from rivals"** (one anecdote), and projected "market share gains" into FY2027 — but the sources caution one anecdote ≠ trend [secondary via Benzinga cloudfront mirror]. **Named displaced competitor / customer not found** — flag as gap.
- **Benchmarks**: No head-to-head 2026 benchmarks (e.g., vs. Arista 800G Etherlink or NVIDIA Spectrum-X) were found in the covered sources. Claims of "boost GPU utilization / improve job completion times" are Cisco marketing without published comparative numbers [official claims; independent verification absent]. Flag as gap.
- **Analyst framing**: Cisco's AI pitch targets the "regulated middle" — enterprise, neocloud, sovereign clouds — where lifecycle support and security matter [secondary via CryptoDaily]. McKinsey-cited figures (Cisco VP Jake Katz blog): $4.7T global DC IT-equipment spend 2025–2030; hyperscalers >60%; neoclouds ~17% growing to >30% in ten years [independent via Network World]. Dell'Oro's Sian Morgan and ZK's Zeus Kerravala are skeptical of Arista's campus-enterprise push — indirect tailwind framing for Cisco in enterprise [independent].
- **Arista counter-moves (2026 context)**: acquired VeloCloud (SD-WAN) from Broadcom; targeting $1.25B campus revenue next year [independent].
- **NVIDIA counter-position**: Spectrum-X Photonics (up to 400 Tbps total, 2,048 ports) and Quantum-X Photonics (144× 800G InfiniBand, CPO) positioned as the AI-native benchmark [secondary].

---

## 10. Event timeline (2026)

| Date | Event | Source |
|---|---|---|
| Feb 10, 2026 | Cisco Live EMEA, Amsterdam: Silicon One G300 (102.4T), new Nexus 9000/8000 systems, 1.6T OSFP + 800G LPO optics, Nexus One + Unified Fabric, native Splunk integration | [official/independent] |
| ~Apr 2026 | Intent to acquire Galileo Technologies (undisclosed) | [secondary] |
| May 4, 2026 | Intent to acquire Astrix Security (~$400M per press; official terms undisclosed) | [independent/secondary] |
| May 13, 2026 | Q3 FY2026 earnings: $15.84B rev; $5.3B AI orders YTD; FY target raised to $9B; ~4,000 job cuts | [independent] |
| June 2–4, 2026 | Cisco Live, Las Vegas: Cisco Cloud Control + AgenticOps/AI Canvas (CA June 2, GA planned July); Agentic Actions beta (Meraki); Digital Twin alpha July; Multicloud Fabric | [independent] |
| June 23, 2026 | CEO: Acacia to grow 200% in FY2026; record 400G/800G coherent quarter | [independent] |
| Aug 12, 2026 | Q4 FY2026 earnings: $17.3B rev (+18%); $4B Q4 AI orders; $9.3B FY AI orders; $4B FY AI revenue; FY27 guide $72.2–73.4B; $7.5B FY27 AI-infra revenue | [independent] |
| Aug 25, 2026 | Cisco + Teleport "Infrastructure Identity" partnership; Cisco largest strategic investor | [secondary] |
| Aug 31, 2026 | AMD/Cisco/HUMAIN JV first segment operational (MI355X) | [independent] |
| Sept 15, 2026 | Cisco AI POD for Splunk announced at Splunk.conf, Denver | [secondary] |
| Sept 17, 2026 | Cisco joins Verizon 6G Innovation Forum | [secondary] |
| ~Sept 21, 2026 | Internet2/NRP: two AI PODs (8× H200 total) deployed | [official via PRN] |

---

## 11. Gaps and uncertainties (explicit flags)

1. **G300 on-chip buffer size**: "industry's largest on-chip buffer" claimed but no MB/GB figure found. Not verified against independent spec.
2. **G500**: no 2026 announcement found. Only G300 confirmed new for 2026.
3. **204.8T**: no Cisco 2026 announcement found; top public density is 102.4T.
4. **CPO**: no Cisco CPO product in 2026 found; Cisco is betting on pluggables (1.6T OSFP, 800G LPO) while NVIDIA/Broadcom lead CPO. Any Cisco CPO roadmap timing is unknown from these sources.
5. **Named hyperscaler customer wins**: Cisco reports AI orders in aggregate; no named hyperscaler logos or named displacement wins verified.
6. **Head-to-head benchmarks** vs. Arista/NVIDIA: none found.
7. **Earnings figures**: sourced from multiple consistent trade-press reports, but the actual Cisco IR release text was not directly read in this pass — treat numbers as independently-reported-from-earnings.
8. **AI POD for Splunk** details: sourced from secondary outlets; original Cisco announcement not directly verified.
9. **Astrix price ($400M)**: press-reported (Calcalist/CTech); Cisco did not disclose. Talks-stage $250–350M also reported. Use "$250–400M range per press; officially undisclosed."
10. **Nexus Dashboard 4.2 AI job-monitoring details**: from a secondary blog; not verified against Cisco docs.
11. **List prices**: only third-party retailer pricing found ($128,731.99 for N9K-C9332D-H2R via Zones [secondary]); no Cisco list prices found.
12. **Internet2/NRP campus names**: not captured in this pass.
13. **R&A partnership** (July 8, 2026, Official Network Supplier for The Open etc.) is real [official] but a sports-hospitality deal, not data-center — excluded from main narrative.

---

## 12. Key sources (verbatim URLs)

- Cisco G300 press release (PDF via q4cdn, official): https://s21.q4cdn.com/812015656/files/doc_news/Cisco-Announces-New-Silicon-One-G300-Advanced-Systems-and-Optics-to-Power-and-Scale-AI-Data-Centers-for-the-Agentic-Era-2026.pdf
- Cisco G300 PR (PR Newswire mirror): https://www.morningstar.com/news/pr-newswire/20260210sf84430/cisco-announces-new-silicon-one-g300-advanced-systems-and-optics-to-power-and-scale-ai-data-centers-for-the-agentic-era
- SiliconANGLE (Feb 10, 2026): https://siliconangle.com/2026/02/10/cisco-targets-high-speed-ai-networking-102-4tbps-chip-liquid-cooled-switches/
- eWeek G300 analysis: https://www.eweek.com/news/cisco-silicon-one-g300-chip/
- SDxCentral (Cisco Live EMEA 2026 rundown): https://www.sdxcentral.com/news/next-gen-switches-servers-everything-unveiled-at-cisco-live-emea-2026/
- CRN (Cisco Live EMEA products): https://www.crn.com/news/networking/2026/cisco-live-amsterdam-2026-the-5-new-networking-security-and-collaboration-products-on-display
- CRN (Cisco Live 2026 Las Vegas — Cloud Control/AI Canvas): https://www.crn.com/news/ai/2026/cisco-unveils-cloud-control-ai-canvas-quantum-security-push-at-cisco-live
- Cisco blog (Nexus 9000 for AI): https://blogs.cisco.com/datacenter/supercharge-your-ai-data-center-infrastructure-with-new-cisco-nexus-9000-series-switches?dtid=oblgzzz000659
- Cisco AI PODs data sheet (official): https://www.Cisco.com/c/en/us/products/collateral/servers-unified-computing/ucs-x-series-modular-system/ai-pods-ds.pdf
- Cisco AI POD for NVIDIA 2-8-9-400 ERA (official CVD): https://www.cisco.com/c/en/us/products/collateral/servers-unified-computing/ai-pod-ucs-c885a-servers-nexus-9364e--sg2-switches.pdf
- Internet2/NRP AI PODs (PR Newswire, ~Sept 21, 2026): https://www.prnewswire.com/news-releases/internet2-and-cisco-add-ai-capacity-to-the-national-research-platform-302886325.html
- eeNews Europe (AI POD for Splunk, Sept 16, 2026): https://www.eenewseurope.com/en/cisco-expands-splunk-ai-with-nvidia/
- Cisco N9332D-H2R data sheet (official): http://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/nexus-9332d-h2r-switch-ds.html
- Barron's (Q4 FY2026 earnings): https://www.barrons.com/articles/cisco-systems-earnings-stock-price-ec1ea4f5
- Trefis (orders-vs-revenue analysis, Sept 17, 2026): https://www.trefis.com/stock/csco/articles/615687/is-cisco-stock-priced-for-orders-that-are-not-yet-revenue/2026-09-17
- The Weighted Average (Q3 FY2026 + layoffs): https://theweightedaverage.com/posts/2026-05-14-cisco-ai-orders-4000-layoffs-networking-boom/
- Cisco investor deck (Feb 19, 2026, official): https://investor.cisco.com/files/doc_presentation/2026/02/Cisco-Investor-Relations-Deck-2-19-26.pdf
- Lightwave (Acacia FY2026 growth, June 23, 2026): https://www.lightwaveonline.com/home/article/55386065/ciscos-ceo-says-acacia-is-set-to-grow-200-in-fiscal-year-2026
- Light Reading (Cisco pluggables/optical, 2026): https://www.lightreading.com/optical-networking/cisco-bets-big-on-pluggables-as-ai-rewrites-the-optical-networking-rulebook
- CRN (Astrix acquisition talks, Apr 14, 2026): https://www.crn.com/news/networking/2026/cisco-eyes-astrix-security-to-lock-down-ai-agents-in-potential-350m-deal-reports
- Network World (Galileo + Astrix analysis): https://www.networkworld.com/article/4158694/cisco-just-made-two-moves-to-own-the-ai-infrastructure-stack.html
- Datamation (AMD/Cisco/HUMAIN JV): https://www.datamation.com/artificial-intelligence/amd-cisco-humain-ai-jv/
- Data Centre Magazine (JV operational status): https://datacentremagazine.com/news/how-will-amd-cisco-and-humain-partner-on-1gw-ai-buildout
- Network World (Cisco 2026 agenda): https://www.networkworld.com/article/4113462/ciscos-2026-agenda-prioritizes-ai-ready-infrastructure-connectivity.html

---

# PART 2 — JUNIPER NETWORKS (post-HPE acquisition)

# Juniper Networks, 2026 — Post-HPE Acquisition Research Report
**Report date:** 22 September 2026 (all figures "as of" this date)
**Provenance tags:** [official] = vendor press release/filing/earnings; [vendor-reported] = vendor claim quoted in media; [independent] = analyst firm/research; [secondary] = media reporting; [unverified] = single weak source / not corroborated. Claims without a tag prefix are the reporter's own synthesis and should be treated cautiously.

---

## 1. HPE–Juniper acquisition status

- **The deal closed on 2 July 2025** — ~18 months after its announcement on 9 January 2024 (Juniper shareholder approval: 2 April 2024). Consideration was ~$14 billion (some outlets cite $13.4B net). Juniper is a wholly owned subsidiary of HPE. [secondary — CRN; The Stack]
- **DOJ settlement (28 June 2025)** cleared the merger just before close. Remedies: HPE must divest its global Instant On campus/branch WLAN business (within 180 days to a DOJ-approved buyer) and must auction a license to Juniper's Mist AIOps source code to competitors. [secondary — NetworkDevicesInc buyer's guide citing the DOJ settlement; corroborated in broad strokes by ainvest reporting]
- **August 2026 court ruling (the item the task notes):** a coalition of state attorneys general (led by Colorado) had challenged the DOJ settlement as politically motivated and insufficient; U.S. District Judge **Casey Pitts** (N.D. Cal.) issued a **41-page ruling in mid-August 2026** dismissing the states' challenge, finding they failed to show the amended final judgment was not in the public interest. This is the final legal clearance: HPE may operate Juniper as a fully integrated entity. [secondary — ainvest, 14 Aug 2026]
- **Integration status:** According to HPE court filings (Dec 2025), HPE moved immediately after closing to integrate employees, products, systems, IP, finance, sales, channel and corporate structures; the business operates as a single **HPE Networking** operating unit. ~4,000 U.S. and ~1,700 non-U.S. former Juniper employees moved onto HPE IT/payroll/benefits; the former Juniper CPO, CSO, CFO, CMO, General Counsel and Chief People Officer are gone. **Rami Rahim** (ex-Juniper CEO) leads the unified HPE Networking business as EVP/President/GM. [official — HPE court filing via appliedantitrust.com]
- **Instant On divestiture status:** Struggling. Reporting says Extreme Networks and Tech Mahindra declined to bid; Fortinet offered only $5M–$15M; one investor group offered $1 for a standalone spinout. Guidance for buyers: do not build new deployments on Instant On. [unverified — single buyer-guide source; DOJ buyer approval status unknown]
- **Earnings commentary:** HPE says the Juniper integration and the "Catalyst initiative" are both running **ahead of schedule**, and raised its Networks-for-AI order target in Q3 FY26 (see §3). [official — HPE earnings calls]

## 2. 2026 product launches

### 2.1 Routing (PTX/MX) — Express 5 / Trio 6
- **Pre-MWC 2026 (announced ~25 Feb 2026):** two new Juniper PTX core router families for AI fabrics, distributed cloud and DCI, all built on the **Express 5** custom ASIC [official/secondary — HPE press release via Network World, Data Center Knowledge, HostingJournalist]:
  - **PTX12000** modular family: **PTX12008** (8 slots, 22RU, up to 345.6 Tbps) and **PTX12012** (12 slots, 32RU, up to 518.4 Tbps), 800GbE port density, **1.6T-ready** platforms, deep buffering, high-radix architecture.
  - **PTX10002** compact fixed-form 2RU routers for metro aggregation, peering, edge and AI data-center networking: up to **28.8 Tbps** throughput, ports up to 800G.
  - Express 5: each chip up to **28.8 Tb/s** (36× 800GbE equivalent); HPE claims **49% better power efficiency** than the previous generation. [vendor-reported]
- **Benchmark claim vs Cisco/Nokia:** Juniper VP Brendan Gibbs claimed Express 5 outperforms Cisco's P100 (19.2 Tb/s, "33% less throughput") and Nokia's FP5. Dell'Oro analyst Shin Umeda: Express 5 = core routing; Trio 6 = edge routing. Cisco declined to comment. [vendor-reported, unchallenged — treat as Juniper claim, not independently verified]
- **MX + Trio 6:** three MX-series routers powered by the **Trio 6** ASIC were announced alongside Express 5 (Oct 2025); Rahim reported "healthy demand" across MX and PTX. [secondary — SDxCentral]
- **MX301 inference-edge router** and **AI-Native Routing** (routers enforcing policy on access to public AI platforms): refreshed/launched at HPE Discover 2026 (June 2026). [secondary — Futurum recap]

### 2.2 Data-center switching (QFX)
- **HPE Discover 2026 (15–18 June, Las Vegas):** refreshed the AI switch lineup [secondary — Futurum]:
  - **QFX5252** — liquid-cooled **scale-up** switch designed for AMD Helios AI racks (see §4, partnerships);
  - **QFX5250** — scale-out switch;
  - **QFX5140** — inference switch.
- **QFX5240** (launched 2025, still the flagship 800G AI fabric platform): Broadcom **Tomahawk 5**-based, 64×800GbE OSFP in 2RU, 51.2 Tbps; positioned as the foundation of HPE's AI data-center fabric with Apstra assurance for AI/ML training, "fast job completion time" for GPU utilization. HPE Store lists multiple SKUs (e.g. QFX5240-64OD, QFX5241-32OD/64OD, AC/DC, ORv3 trays). [official — HPE Store]
- Junos EVO AI-ML fabric software features now documented for the QFX5xxx line: **BGP Global Load Balancing (GLB)** for AI/ML elephant-flow congestion, **Configurable FlowSet table in DLB flowlet mode** (QFX5130/5220/5230/5240/5700), reactive path rebalancing, selective DLB via firewall filters — collectively the **Ops4AI** (Operations for AI) extensions to Junos + Apstra. [official — Juniper docs (24.4 release notes)]

### 2.3 Mist AI / campus & branch (HPE Mist)
- **HPE Mist** (renamed from Juniper Mist) **now manages HPE Networking CX switches** — the Juniper AIOps engine moved into the Aruba access layer. **Marvis** — the self-driving framework — is **embedded into Aruba Central**. Strategy: "build once, deploy twice," rolling out from **Q1 2026**. [secondary — NAND Research Discover 2026 recap]
- **Discover Barcelona (Dec 2025):** Juniper's **Large Experience Model** (analyzes data from Zoom/Teams etc.) added to Aruba Central; Aruba's **Agentic Mesh** coming to Mist, improving issue detection, root-cause and remediation. [secondary — SiliconANGLE]
- **HPE Networking 723H** — first unified Juniper×Aruba product, shipped **6 May 2026**: a three-radio 2×2 **Wi-Fi 7 (802.11be)** wall-plate/hospitality AP that boots into **either Mist or Aruba Central** management (auto-detects at claim; existing licenses honored across platforms). HPE says **all future wireless hardware will follow this dual-platform strategy**. [secondary — The Register; Tech Field Day]
- **"Self-Driving Networks"** launch (May 2026): autonomous workflows executed without operator approval — dynamic wireless capacity optimization (e.g. re-provisioning capacity for an all-hands gathering), VLAN mismatch/missing-VLAN correction, rogue DHCP server neutralization, RF channel retuning for interference, client roaming refinement, end-to-end latency measurement from client association to cloud, and **automatic avoidance of frequencies reserved for military/priority users** (policy-driven channel allocation beyond standard DFS). Humans can remain in the loop. [secondary — The Register, 8 May 2026]
- **Market signals:** HPE Q2 FY26 earnings — **Wi-Fi 7 AP sales up 7×**; campus & branch orders hit a record, +20% normalized, with multimillion-dollar deals across verticals. [official — HPE Q2 earnings call]
- HPE showcased its wireless infrastructure role at the **2026 Winter Olympics** (served over 1 million clients across 5/6 GHz bands); 6 GHz adoption at ~60% of HPE AP shipments; Wi-Fi 7 at 10–15% adoption per HPE. [vendor-reported — Tech Field Day]
- Older Marvis/Mist capabilities (Minis digital twins, Wired/WAN Assurance) remain current; no discrete 2026 Mist platform re-launch found beyond the HPE Mist rebrand and cross-pollination work.

### 2.4 Security
- **SRX400 series** firewalls launched at **RSA Conference 2026** (24–26 March, Moscone), available Q2 2026 — carrier-grade security for smaller/space-constrained edge sites, compact form factor, hardware tamper protection, part of the hybrid mesh firewall. HPE credited the launch with security orders +15–19% (normalized) in Q2 FY26. [official — HPE press release; earnings call]
- At Discover 2026: **quantum-safe SRX4700** (AI Predictive Threat Prevention), **SASE Orchestrator** (unifies SD-WAN and SSE in one console — due later 2026), **SASE Copilot**, **AI Firewall**, Universal ZTNA extended to non-human/agentic identities, Security Director Copilot. [secondary — Futurum]

### 2.5 Apstra / Junos OS
- **Apstra:** no discrete 2026 Apstra release found in this research. It remains the intent-based networking layer (Apstra Data Center Director) underpinning HPE's AI fabric management alongside Ops4AI; the QFX5240 store page cites Apstra Data Center Director for daily-operation assurance of AI training fabrics. [official — HPE Store; no 2026-dated version found — gap flagged]
- **Junos OS:** latest documented AI-ML feature set is in Junos EVO 24.4-era release notes (GLB, DLB FlowSet, reactive path rebalancing, rdma-opcode support); no new named Junos release found for 2026. Marketing emphasis is on "AI-optimized Junos software (e.g. AI load balancing)" rather than a versioned release. [official — Juniper docs; gap flagged]

### 2.6 ACX
- No 2026 ACX hardware announcements found. **Gap flagged** — if ACX launches occurred, they were not surfaced by this search pass.

## 3. AI networking revenue figures & customer wins (2026)

HPE fiscal year ends 31 October. Juniper figures are reported inside HPE Networking.

### 3.1 Q2 FY2026 (quarter ended 30 April 2026; reported ~June 2026)
- Total HPE revenue **$10.7B, +40% y/y** (above $10B high-end guidance); GAAP net income $624M (vs $1B loss a year ago); non-GAAP EPS $0.79, +108%. [official]
- **Networking revenue: $2.7B, +148.2% reported / +10% normalized** (normalized to include pre-acquisition Juniper y/y); network operating margin **21.6%** (down from 25.0% on integration costs); Cloud & AI revenue $7.7B, +22.9%. [official]
- Sub-numbers: campus & branch networking revenue **$1.3B**; data-center networking revenue **$320M**; routing revenue **$775M**; campus & branch orders record +20% normalized; enterprise DC switching orders +~20% normalized; routing orders +~30% normalized; security orders +15–19% normalized; Wi-Fi 7 AP sales 7×. [official — earnings call transcript via Fool/Constellation]
- **Networks for AI order target raised to ≥$2B cumulative by fiscal year-end** (June 2026 guidance). [official]
- AI systems: $1.8B new orders in the quarter; **$16.4B cumulative AI bookings**; AI systems backlog record **$5.9B**. [official]

### 3.2 Q3 FY2026 (reported early September 2026)
- Total revenue **$12.2B, +34% y/y** (record); GAAP EPS $1.06 (vs $0.21); non-GAAP EPS $1.11 (above $0.88–0.93 outlook); non-GAAP gross margin 40.4%. FY26 revenue-growth outlook raised to **34–37%**; FCF floor raised to $3.75B; FY27 revenue growth guided **13–17%**. [official — HPE Q3 FY26 release via multiple outlets]
- **Networking revenue: $2.9B, +74.9% reported / +10% normalized**; operating margin **22%**; normalized networking **orders +36%** (well ahead of revenue — demand exceeds shipments; supply constraints are limiting conversion, with purchase commitments more than doubled sequentially). [official — via Zacks, Constellation, ConvergeDigest]
- Sub-numbers: campus & branch **$1.44B** (+31% reported); data-center networking **$382M** (+112%); routing **$788M** (+270% — includes the Juniper routing line); DC switching & routing orders up at high double-digit rates. [official]
- **Networks for AI: $700M orders in Q3; $2.2B cumulative; FY26 target raised to $2.5–3.0B.** Combined AI backlog (AI Systems + Networks for AI): **$7.6B**. [official]
- **Customer win: Oracle** — gigawatt-scale, multi-gigawatt AI cloud buildout deploying HPE Juniper Networking switches and routers: **QFX switching for "Scale Out" + PTX routing for "Scale Across."** Announced on the Q3 earnings call. [official — CEO Antonio Neri via ConvergeDigest]
- HPE said the upcoming AMD Helios platform will incorporate HPE Juniper Scale-Up switches — but the Helios opportunity is **excluded** from the current FY27 networking growth framework. [official — ConvergeDigest]

### 3.3 Customer deployments (non-OEM)
- **Oracle AI cloud** (gigawatt scale, Sept 2026) — see above. [official]
- **2026 Winter Olympics** infrastructure — HPE networking (>1M clients). [vendor-reported]
- Neocloud/CSP AI-fabric deployments are cited qualitatively ("AI factories and inferencing… hyperscalers, neoclouds") but few are named publicly; PTX orders up ~30% normalized on cloud-service-provider deployments in Q2. [official/secondary]
- **Gap:** no named per-quarter Juniper-only AI revenue split post-acquisition; HPE reports "Networks for AI" orders as the AI proxy.

## 4. AI data-center networking strategy

- **Ethernet for AI / open standards:** HPE Juniper's strategy is explicitly standards-based Ethernet: Junos AI load balancing (GLB/DLB), multivendor fabric management (Apstra), and Mist AIOps layered on merchant silicon — Juniper's message is "secure, high-performance, multivendor, lower TCO" vs. closed stacks. [vendor-reported — Praveen Jain quote in Broadcom Tomahawk 6 PR]
- **Ultra Ethernet / UEC participation:** not confirmed. Juniper praised Broadcom's Tomahawk 6 as an "open, **UEC-compliant** architecture," but **no source in this pass confirms Juniper/UEC Consortium membership**. [flagged — unverified]
- **Congestion management (DCQCN / RoCEv2):** Junos EVO docs reference RoCE/rdma-opcode support and AI-ML load-balancing features, but **no 2026-dated Juniper announcement on DCQCN or RoCEv2 congestion features** was found. [gap flagged]
- **400G/800G portfolio:** QFX5240/5241 (64×800G / 32×800G, Tomahawk 5), PTX10002 (800G fixed), PTX12000/PTX10000 (800GbE, 1.6T-ready platforms); 1.6T readiness is a 2026 theme but no 1.6T ports ship yet. [official/secondary]
- **Partnerships:**
  - **Broadcom:** deepening. Dec 18, 2025 — HPE announced the **first AMD "Helios" AI rack-scale architecture** with integrated **scale-up Ethernet built with Broadcom**: purpose-built HPE Juniper Networking scale-up switch + software, using Broadcom's **Tomahawk 6** (102.4 Tbps), based on the open **Ultra Accelerator Link over Ethernet (UALoE)** standard, supporting trillion-parameter training. HPE claims **first OEM to productize a 100% liquid-cooled Tomahawk 6 switch** (from the Q2 FY26 earnings call). The Tomahawk 6 launch PR quotes Juniper SVP Praveen Jain (AI Clusters & Cloud Ready Data Center). Broadcom's industry-first **800G Thor Ultra NIC** is positioned alongside the HPE Juniper AI-optimized fabric. [official/secondary]
  - **AMD:** co-launch of the Helios rack-scale architecture (scale-up Ethernet over UALoE). [official]
  - **NVIDIA:** HPE "AI Factory" and **AI Grid with NVIDIA** (announced at NVIDIA GTC, March 2026): HPE Juniper PTX/MX routers + **800G ZR+ coherent optics**, MACsec, IP/MPLS for scale-across/DCI; HPE AI Factory with **Spectrum-X Ethernet**, BlueField-3, Blackwell GPUs, Vera Rubin NVL72 on the roadmap; AI Factory Lab in Grenoble (announced Dec 1, 2025, opened Q2 2026); a "3-2-1 integration strategy" (three silicon platforms from two companies into one fabric). HPE says the NVIDIA relationship is a two-way dependency: HPE adds integration, sovereignty, financing, GSI co-sell. [official/secondary — businesswire; Futurum]
- **Scale-out / scale-up / scale-across taxonomy:** HPE now frames AI networking as scale-out (QFX, e.g. Oracle), scale-up (Tomahawk 6-based switch for Helios), scale-across/DCI (PTX12000). [official]

## 5. Positioning vs Cisco in AI data-center networking (2026)

- **Market data (IDC, Q2 2026)** via Next Platform (Sept 20, 2026) and kad8:
  - Global Ethernet switch revenue **$18.9B** in Q2 2026 (+64.5% y/y for the data-center segment, $12.3B). [independent — IDC via Next Platform/kad8]
  - **Cisco: $5.43B, +36.2%** (28.7% global share); ~$2.2B from data-center switches; router business $1.6B (+31.6%). Largest vendor overall. [independent]
  - **NVIDIA: ~$2.5B (per kad8, +181% y/y; ~20% of DC Ethernet) — but Next Platform cites $3.86B growing 2.8×.** The two figures appear to use different scopes; **flagged as unresolved.** [independent — discrepancy noted]
  - **Arista: $2.53B, +37.6%** (13.4% global; 18.7% DC). Arista launched a modular router at >115 Tbps in early 2026 aimed at AI. [independent — IDC via Next Platform; Arista launch via Think Insights]
  - **HPE (incl. Juniper): $1.15B Ethernet, +6.1%** — "a lot of incremental business for HPE, whose Ethernet business was largely wireless under Aruba." Huawei $1.55B (+28.6%); ODMs ~$3.56B (+2.43×). [independent — Next Platform's IDC augmentation]
  - **Reading:** HPE-Juniper is #4 by Ethernet revenue (behind Cisco, NVIDIA/Arista, Huawei) but is the only vendor with full campus-to-DC-to-routing plus compute/storage; its growth lags the AI-cluster leaders (NVIDIA, Arista) in DC switching share.
- **Silicon positioning:** Juniper claims Express 5 (28.8 Tb/s) beats Cisco P100 (19.2 Tb/s) by ~33% throughput — a vendor claim Cisco declined to confirm or deny. [vendor-reported]
- **Analyst commentary:**
  - Futurum (Discover 2026 recap): "the networking story is the strongest, most defensible part of HPE's hand"; honest tension is the two coexisting management planes (HPE Mist vs Aruba Central); key tests = self-driving delivery in brownfield, security story above the network, NVIDIA dependency. [independent]
  - ACG's Ray Mota: "HPE delivers the architectural and operational foundation service providers need to fully participate in the AI value chain." [independent quote]
  - EMA's Shamus McGillicuddy: Juniper was showing more value than anyone else; HPE's acquisition centers on that. [independent]
  - Think Insights industry analysis: HPE-Juniper is a new full-stack competitor "explicitly positioned to challenge Cisco and Nvidia in AI-native networking"; Arista retains hyperscale/DC advantage; white-box/SONiC competes on price. [independent]
  - ETR AI survey cited by Futurum: ~21% of orgs building AI apps use HPE for hosting/deployment (and plan to stay), 14% considering — "most of the field is still open." [independent]
  - HPE's own claim (Feb 2026, MWC): "I don't think you could find one company who has a leadership in compute, storage, security and networking across the board" — the full-stack pitch vs Cisco/Arista. [vendor-reported]
- **Customer wins vs Cisco (2026):** Oracle's gigawatt-scale HPE Juniper win is the marquee 2026 reference; Wi-Fi 7 AP sales 7× and campus record orders suggest campus displacement momentum; no 2026-dated named hyperscale DC wins against Cisco surfaced. [official on Oracle; gaps flagged]

## 6. Optics and co-packaged optics (CPO), 2026

- **No Juniper-specific CPO product announcement found in 2026.** This is a notable absence given Broadcom's 2026 CPO push. [explicit gap]
- **Adjacent CPO facts (context):**
  - Broadcom's **Tomahawk 6** (102.4 Tbps) ships with a **native CPO variant** (built on TSMC photonics tech); Juniper builds on Tomahawk 6 for its AI switches and endorsed the chip's "UEC-compliant architecture" and CPO direction. [official — Broadcom PR; vendor-reported Juniper quote]
  - Industry context: TSMC's first true CPO implementation (COUPE photonics) entering production in 2026; NVIDIA's Spectrum-X Ethernet Photonics in production alongside Vera Rubin; 1.6T optical modules ramping in 2026. [secondary]
- **Juniper optics that did move in 2026:** **800G ZR+ coherent optics** on PTX/MX for distributed AI-factory DCI (HPE AI Grid / GTC 2026 announcements; also the AI Factory Grid architecture with NVIDIA). [official/secondary]
- Broadcom's 800G **Thor Ultra NIC** is positioned with the "HPE Juniper Networking AI-optimized fabric" — end-to-end fabric messaging rather than a Juniper transceiver product. [official — Broadcom PR]

## 7. Key uncertainties & gaps

1. **UEC membership:** no 2026 source confirms Juniper in the Ultra Ethernet Consortium; only that Juniper endorses UEC-compliant merchant silicon. [unverified]
2. **CPO product:** no Juniper CPO product announcement; strategy appears to ride Broadcom's CPO roadmap. [gap]
3. **Apstra 2026 version:** no discrete 2026 Apstra release found. [gap]
4. **Junos OS 2026 release:** no new named Junos version for 2026 found; latest AI-ML docs are 24.4-era. [gap]
5. **ACX 2026:** no announcements found. [gap]
6. **DCQCN/RoCEv2 2026 news:** none found (only pre-existing Junos EVO RoCE/AI-load-balancing docs). [gap]
7. **NVIDIA IDC revenue discrepancy** ($2.5B vs $3.86B for Q2 2026) unresolved. [flagged]
8. **Deal value** cited as both $14B and $13.4B (likely gross vs net of cash); used "$14B (some outlets $13.4B)". [flagged]
9. **Instant On divestiture** details (buyer, timing) rest on a single buyer-guide source; DOJ-approved buyer unknown. [unverified]

## 8. Sources consulted (all via web search/fetch, Sept 2026)
- CRN (deal close); The Stack (close/analyst quotes); ainvest (Aug 2026 ruling, Judge Pitts); appliedantitrust.com (HPE court filing, Dec 2025); networkdevicesinc.com (buyer guide); NAND Research (Discover 2026 recap); Futurum Group (Discover 2026 analysis, 24 June 2026); SiliconANGLE (Discover Barcelona, Dec 2025); The Register (723H AP, 8 May 2026); Tech Field Day (Wi-Fi 7 updates); Network World (PTX12000, Express 5); Data Center Knowledge (PTX, MWC 2026); Fierce Network (MWC AI); HostingJournalist (PTX); SDxCentral (Express 5 vs Cisco P100/Nokia FP5); stocktitan (HPE MWC PR); HPE Store UK (QFX5240 SKUs); Juniper.net docs (Junos EVO AI-ML, GLB/DLB); blocksandfiles.com (Q2 FY26); Fool (Q2 FY26 earnings transcript); control.vg (Q2 FY26); Constellation Research (Q2 & Q3 FY26); ConvergeDigest (Q3 FY26, Oracle win); Zacks (Q3 FY26); marketbusinessnews (Q3 FY26); skn-finance (Q3 FY26); dividendinformer (Q3 FY26); Next Platform (IDC Q2 2026 Ethernet data, 20 Sept 2026); kad8 (IDC Q2 2026); Think Insights (industry analysis); ad-hoc-news.de (analyst coverage); Broadcom PR via GlobeNewswire (Tomahawk 6); cxotoday.com (AMD Helios, Dec 2025); businesswire (HPE AI Grid with NVIDIA, 17 Mar 2026); business-news-today/expresscomputer (HPE-NVIDIA sovereign AI); CRN/stocktitan/chartmill/barchart/nomios/saudishopper (SRX400, RSA March 2026); markets.financialcontent.com/urdupure.com/github silicon-photonics research (CPO context).

---

# PART 3 — CROSS-VENDOR SYNTHESIS

*(Assembler-added section; research as of September 22, 2026.)*

---

## 9. Vendor positioning in AI data-center networking (2026)

### 9.1 Market snapshot — Ethernet switch revenue, Q2 2026

| Vendor | Q2 2026 switch revenue | Global Ethernet share | Data-center share | Notable 2026 move |
|---|---|---|---|---|
| Cisco | $5.43B (+36.2% y/y) | 28.7% (#1) | ~$2.2B (~18%) | Silicon One G300 102.4T; FY26 $9.3B hyperscaler AI infra orders |
| NVIDIA | ~$2.5B (+181% y/y; Next Platform cites $3.86B — scope discrepancy flagged) | — | 20.4% (#1 branded DC, IDC) | Spectrum-X CPO + Spectrum-X Ethernet Photonics in production with Vera Rubin |
| Arista | $2.53B (+37.6% y/y) | 13.4% | 18.7% | Tomahawk 6-based 7060XE7 up to 1.6T; full-year 2026 revenue guided >$10B |
| HPE (incl. Juniper) | $1.15B (+6.1%) | — | ~#4 | Oracle gigawatt-scale win (QFX scale-out + PTX scale-across); Networks-for-AI $2.2B cumulative orders |
| Huawei | $1.55B (+28.6%) | 8.2% | — | SP strength, China/emerging markets |

[independent — IDC via Next Platform (Sept 20, 2026) and kad8; NVIDIA figure discrepancy flagged in both drafts]

- Dell'Oro Q2 2026 AI back-end report: Ethernet AI back-end sales dominated by Celestica (white box) then NVIDIA; Arista third; **Cisco gained the most share, ranked fourth**. 800G was the vast majority of shipments; 1.6T began sampling, ramping H2 2026. AI back-end switch sales surpassed front-end sales for the first time in Q2 2026. [independent — Dell'Oro]
- Total Ethernet switch market: $18.9B in Q2 2026 (+43.4% y/y); DC segment $12.3B (+64.5%). [independent — IDC]
- thecodew analysis (Aug 2026): NVIDIA's DC Ethernet share rose from <4% to ~21.5% in Q1 2026, ahead of Arista (~19%) and Cisco (~14%). [secondary — treat figures as approximate]

### 9.2 Strategic comparison: Cisco vs HPE-Juniper

| Dimension | Cisco | HPE-Juniper |
|---|---|---|
| Core AI silicon | Silicon One G300 102.4T (in-house), plus Spectrum-X silicon support in same chassis | Merchant: Broadcom Tomahawk 6 (incl. CPO variant) for scale-up; Express 5 custom ASIC for routing |
| Fabric scale taxonomy | Scale-out focus; "Intelligent Collective Networking" | Scale-out (QFX), scale-up (TH6 over UALoE for Helios), scale-across/DCI (PTX12000) |
| Unified management | Nexus One + Unified Fabric (Feb 2026); Cloud Control / AgenticOps AI Canvas | HPE Mist AIOps + Aruba Central; "build once, deploy twice" (rollout Q1 2026) |
| UEC role | Member | Endorses UEC-compliant silicon; formal membership **unconfirmed** [unverified] |
| CPO posture | No 2026 CPO product; pluggable bet (1.6T OSFP, 800G LPO) | No Juniper CPO product; rides Broadcom TH6-Davisson CPO roadmap |
| Named 2026 hyperscaler wins | None named publicly (orders in aggregate) | Oracle gigawatt-scale AI cloud (QFX + PTX) |
| FY2026 AI networking figures | $9.3B hyperscaler AI infra orders (4.5× FY25); ~$4B revenue; FY27 guide $7.5B | Networks-for-AI: $2.2B cumulative orders Q3; FY26 target $2.5–3.0B; combined AI backlog $7.6B |

### 9.3 Optics & co-packaged optics — 2026 context

- **CPO entered volume production in 2026** (NVIDIA + Broadcom), though optical-engine/package supply remains a constraint risk. TrendForce: NVIDIA Spectrum-X CPO switch (co-developed with TSMC, COUPE packaging) and Broadcom **Bailly CPO** switches are in mass production, expanding late 2026. Spectrum-X CPO delivers up to 400 Tb/s; Bailly CPO cuts power up to 70% vs pluggables and completed deployment validation with hyperscalers including Meta. [secondary — TrendForce via electronics360]
- **Broadcom Tomahawk 6 – Davisson (TH6-Davisson)**: third-gen CPO Ethernet switch, industry's first 102.4 Tbps optically enabled switching, 16× 6.4T optical engines on TSMC COUPE; 70% lower optical-interconnect power (>3.5× vs pluggables); improved link-flap stability. [official — Broadcom]
- **NVIDIA**: Spectrum-X Ethernet Photonics in production alongside Vera Rubin generation; Quantum-X800 CPO InfiniBand platform at 115.2 Tbps; per-port power 30W→9W vs pluggables. [secondary]
- **Cisco's 2026 stance**: 1.6T OSFP + 800G LPO pluggable modules launched with G300 (50% lower module power, −30% switch power); Acacia shipped >750k 400G + 40k 800G coherent pluggables (record quarter; CEO says 200% FY2026 growth); new Open Transport 3000 multi-rail line system and NCS 1014 800G transponder card. **No Cisco CPO product found in 2026** — Cisco is betting on pluggables while NVIDIA/Broadcom lead CPO. [official/secondary — Cisco draft]
- **Juniper's 2026 stance**: no Juniper CPO product; CPO exposure via Broadcom Tomahawk 6; optics moves in 2026 were **800G ZR+ coherent** on PTX/MX for DCI (HPE AI Grid). [official/secondary — Juniper draft]
- Trend toward 1.6T: 1.6T optical modules ramping in 2026; thermal management of 1.6T pluggable systems pushing adoption of SiPh/CPO designs. [secondary]

### 9.4 Master timeline (January → September 2026)

| Date | Event |
|---|---|
| 2026-02-10 | Cisco announces Silicon One G300 (102.4T) at Cisco Live EMEA Amsterdam [official] |
| ~2026-02-25 | HPE announces PTX12000/PTX10002 Express 5 routers pre-MWC (MWC: Mar 2–5) [official/secondary] |
| 2026-03-17 | HPE AI Grid with NVIDIA announced at NVIDIA GTC (PTX/MX + 800G ZR+, Spectrum-X AI Factory) [official/secondary] |
| 2026-03-24–26 | Juniper SRX400 firewalls launched at RSA Conference [official] |
| ~2026-04 | Cisco acquires Galileo (AI observability, terms undisclosed) [secondary] |
| 2026-05-04 | Cisco announces Astrix Security acquisition (press-reported ~$400M, officially undisclosed) [secondary/unverified price] |
| 2026-05-06 | HPE ships 723H Wi-Fi 7 AP — first unified Juniper×Aruba product (dual Mist/Central) [secondary] |
| 2026-05-08 | HPE "Self-Driving Networks" autonomous workflows launch [secondary] |
| 2026-06-02–04 | Cisco Live Las Vegas: Cisco Cloud Control / AgenticOps + AI Canvas [official/secondary] |
| 2026-06-15–18 | HPE Discover: refreshed AI switch lineup (QFX5252 liquid-cooled scale-up, QFX5250, QFX5140), quantum-safe SRX4700, SASE Orchestrator [secondary] |
| 2026-06-01 | CoreWeave first Vera Rubin rack (context) [from step-5 track A] |
| ~2026-07 | CPO switches (NVIDIA Spectrum-X, Broadcom Bailly) in volume production per TrendForce [secondary] |
| 2026-08-14 | Judge Casey Pitts dismisses states' challenge to HPE–Juniper DOJ settlement (final legal clearance) [secondary] |
| 2026-08-25 | Cisco + Teleport identity partnership [secondary] |
| 2026-08-31 | AMD/Cisco/HUMAIN Saudi AI zone operational on MI355X (Luma AI anchor) [secondary — Cisco draft] |
| 2026-09-15 | Cisco AI POD for Splunk announced at Splunk.conf Denver [official/secondary] |
| 2026-09-17 | Cisco joins Verizon 6G Forum [secondary] |
| 2026-09-20 | Next Platform: IDC Q2 2026 Ethernet data ($18.9B, +43.4%) [independent] |
| 2026-09-22 | Report date |

### 9.5 Master verification log (combined)

1. Cisco earnings-sourced figures ($9.3B orders, $7.5B guide) rest on consistent trade-press coverage, not the IR release text itself — Cisco draft §11.
2. Silicon One G300 on-chip buffer size undisclosed.
3. No Cisco G500 / 204.8T announcement found in 2026.
4. No Cisco CPO product found in 2026 — stance is pluggables.
5. Cisco Astrix price ($400M) press-reported, officially undisclosed.
6. Cisco claims AI Nexus orders +85% sequentially and "market share gains"; one displacement anecdote unverified.
7. Juniper UEC membership unconfirmed.
8. No 2026 Juniper CPO product; strategy rides Broadcom's roadmap.
9. No discrete 2026 Apstra release found.
10. No new named Junos OS version for 2026 found (latest AI-ML docs 24.4-era).
11. No 2026 ACX announcements found.
12. No 2026 Juniper DCQCN/RoCEv2 announcement found.
13. IDC Q2 2026 NVIDIA figure discrepancy: $2.5B (kad8) vs $3.86B (Next Platform) — scope unresolved.
14. HPE–Juniper deal value cited as $14B and $13.4B (likely gross vs net).
15. Instant On divestiture details rest on a single buyer-guide source (Fortinet $5–15M offer, Extreme declined) — buyer approval status unknown.
16. TrendForce CPO "mass production" claims are market-research reporting, not vendor production disclosures — treat as secondary.
17. thecodew market-share figures (NVIDIA 21.5% Q1 2026 etc.) are approximate/secondary.
18. Luma AI / HUMAIN Saudi JV (100 MW → 1 GW) operational status per vendor/PR reporting — secondary.
19. Cisco vs Arista vs HPE-Juniper: no 2026 head-to-head AI-fabric benchmarks located — comparative performance claims are vendor-reported only.

### 9.6 Collection metadata

- **Report file:** `~/workspace/rag_collect/etape6_trackA_cisco_juniper.md`
- **Research date:** 2026-09-22 (all figures "as of" this date unless noted)
- **Coverage window:** January 2026 → September 22, 2026
- **Tracks:** Part 1 Cisco (261 lines), Part 2 Juniper (post-HPE acquisition), Part 3 cross-vendor synthesis
- **Method:** read-only web research (browser search/fetch); no live-browser navigation; no outward sends
- **Provenance legend:** [official] = vendor press release/blog/datasheet/earnings | [vendor-reported] = vendor claim quoted in media | [independent] = analyst firm/established trade outlet analysis | [secondary] = media reporting | [unverified] = single weak source
- **Working drafts retained:** `_draft_etape6_cisco.md`, `_draft_etape6_juniper.md` in `~/workspace/rag_collect/`
