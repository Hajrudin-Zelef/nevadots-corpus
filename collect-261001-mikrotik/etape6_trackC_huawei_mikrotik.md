# Step 6 — Track C: Huawei + MikroTik (Networking Hardware)
## Research report (English) — collected September 22, 2026

**Project:** RAG data-collection, Step 6 (Réseau & sécurité), Track C
**Coverage window:** January 1, 2026 → September 22, 2026
**Scope:** Huawei networking portfolio (data-center switches, AI fabric, international status), MikroTik 2026 launches (routers/switches, RouterOS v7), switch pricing trends (25G/100G/400G), open-source NOS news (brief)

### Provenance legend
- **[official]** — vendor's own site, press release, or documentation.
- **[vendor-reported]** — figure claimed by the vendor without independent audit.
- **[independent]** — reputable third-party press (IDC, Dell'Oro, The Register, SemiAnalysis, Telegeography) or independent measurement.
- **[secondary]** — lower-tier press, marketplace listings, blogs, community coverage; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

---

## 1. HUAWEI — 2026 networking portfolio

### 1.1 HUAWEI CONNECT 2026: Stellar AI Fabric upgrade (Sep 21, 2026)

At HUAWEI CONNECT 2026 (Shanghai), Huawei hosted the **AI DC Innovation Summit** ("Leading AI DC Innovation, Shaping the Agentic World", 600+ industry leaders and researchers) and unveiled an upgraded **Stellar AI Fabric Solution** for production AI clusters **[official — https://www.prnewswire.com/in/news-releases/huawei-connect-2026--huawei-upgrades-stellar-ai-fabric-solution-to-build-efficient-ai-computing-production-networks-302884451.html]**.

Software/management upgrades:
- **Rock-Solid Architecture 2.0**, **StarryWing Digital Map 2.0** (network digital twin), **Hyper-Converged Fabric (HCF)**, **Network Packet Load Balancing (NPLB)**.
- **NetMaster network AI agent**: automates root-cause analysis for **95% of network faults within minutes** (vendor-reported) to maintain production uptime. Keynote by Erick Zhang, President of Data Center Network Domain at Huawei Data Communication Product Line: "AI is turning traditional data centers into compute factories… the underlying network fabric directly dictates token throughput and overall compute yield" **[official]**.

Two new switch families launched for high-density computing:
- **CloudEngine SF9300 (UBG switch)** — two-layer multi-plane architecture that cuts buildout costs by **30%**; simplified UB forwarding lowers end-to-end latency by **40%**; **Link-Layer Retransmission (LLR)** executes microsecond-level recovery, eliminating packet loss during link flapping **[official, vendor-reported]**.
- **CloudEngine XH9300 (NPO switch)** — "Near-Packaged Optics" switch delivering proprietary **100T and 51.2T** (switching capacity figures); centralized light source with proprietary **3.2T Optical Engine (OE)** lowers interconnect power draw by **40%**; eliminating the optical DSP (oDSP) stage cuts forwarding latency by **26%**; snap-in pluggable optical engines speed up maintenance **10×** **[official, vendor-reported]**.

Standards and research deployments:
- The **Beijing FinTech Industry Alliance, Huawei, and 17 partners** released the *Technical Specification for Network High Availability of Financial Data Center*, defining technical baselines and testing methods across two categories and five network types for intra-DC and cross-DC environments (architectural redundancy, fault recovery, proactive prevention) **[official]**.
- **IHEP** (Institute of High Energy Physics, Chinese Academy of Sciences): deploying the Stellar AI Fabric lifted computing efficiency by **over 10%**; network-wide load balancing resolved training traffic imbalances for deep-space signal analysis workloads (per Computing Center Director Qi Fazhi) **[official, vendor-reported]**.

### 1.2 AI data-center networking: CloudMatrix 384

Huawei's AI cluster networking centers on the **CloudMatrix 384** supernode: 384× Ascend 910C + 192× Kunpeng CPUs, connected via Huawei's self-developed **Unified Bus (UB)** in a fully non-blocking all-to-all interconnect — a deliberate alternative to copper/NVLink-style scale-up **[independent — https://semianalysis.com/2025/04/16/huawei-ai-cloudmatrix-384-chinas-answer-to-nvidia-gb200-nvl72/; secondary — https://chinaresearchcollective.substack.com/p/huawei-ascend-cloudmatrix-384-supernode]**.

- **~289–300 PFLOPS (BF16)**, **~48 TB HBM** total, 16 racks; UB bandwidth **196 GB/s per NPU** (vs PCIe 5.0's 64 GB/s); 7-layer sub-plane design, each layer an independent full-mesh **[secondary]**.
- Optical interconnect: **6,912× 400G SiPh LPO (Linear Pluggable Optics) modules** per pod — 5,376 for the vertical (scale-up) expansion network, 1,536 for the horizontal (RDMA scale-out) network; 3,168 optical fibers; each NPU gets 2.8 Tbps interconnect bandwidth (7× 400G transceivers per chip); flat single-tier topology with ~16,800 modular switch elements using cell-spraying (mechanism similar to Broadcom Jericho3 + Ramon3) **[independent/secondary — SemiAnalysis via fibermall.com; https://www.prnewswire.com — Huawei Connect 2025 materials]**.
- Huawei positions LPO as a power-saving measure vs DSP-based optics; tradeoff noted by analysts: higher transceiver count raises cost, power, airflow, and reliability concerns, mitigated by fault-tolerant training software **[independent — SemiAnalysis]**.
- First deployment context: Inner Mongolia Ulanqab Data Center (April 2025 launch); 2026 role: domestic compute foundation for models such as DeepSeek V4 Pro **[secondary]**.

### 1.3 Enterprise Wi-Fi 7 leadership (Mar 2026)

- **March 17, 2026**: Huawei announced it ranked **No. 1 both globally and in China** in enterprise-class **Wi-Fi 7 market share and shipments** per IDC's *Worldwide Quarterly Wireless LAN Tracker (2025 Q4)* — the **second consecutive year** of leadership since IDC began publishing Wi-Fi 7 statistics in 2024 **[official — https://e.huawei.com/en/news/2026/solutions/enterprise-network/wi-fi7-market-share-shipments; independent — IDC]**.
- **2M+ enterprise-class Wi-Fi 7 products shipped**; Huawei is also No. 1 globally in Wi-Fi 7 standards contributions **[official, vendor-reported]**.
- Background: world's first enterprise-class Wi-Fi 7 AP (2022); first commercial Wi-Fi 7 network deployment (2023, Tolly-tested); industry's first complete Wi-Fi 7 portfolio for all scenarios (2024); world's first enterprise-class Wi-Fi CERTIFIED 7 certificate **[official]**.

### 1.4 Market position: Q2 2026 results (IDC, Dell'Oro)

**IDC — global Ethernet switch market, Q2 2026** (reported ~Sep 21, 2026) **[independent — https://www.idc.com/resource-center/blog/ethernet-switch-market-surges-43-4-to-18-9b-in-2q26-as-ai-infrastructure-demand-drives-record-datacenter-spending/; https://www.kad8.com/network/global-ethernet-switch-market-hits-18.9-billion-usd-in-q2-2026/]**:
- Worldwide Ethernet switch revenue hit **$18.9B, +43.4% YoY** — AI infrastructure demand driving record datacenter spending.
- **Huawei: ~$1.5B Ethernet switch revenue, +28.6% YoY, 8.2% global market share.**
- **Huawei router revenue: $1.3B, +21.6% YoY, 29.9% global router market share** — "underscoring continued strength in service provider networking, particularly in China and select emerging markets."
- IDC's three signals: (1) AI cluster construction is a top driver of datacenter network spend; (2) east-west GPU/server traffic makes bandwidth/latency directly affect accelerator utilization; (3) the fabric is becoming part of the AI compute system.

**Dell'Oro — High End Routing and Aggregation, Q2 2026** (reported ~Sep 10, 2026) **[independent — https://www.prnewswire.com/news-releases/high-end-routing-and-aggregation-market-grew-25-percent-in-2q-2026-according-to-delloro-group-302872936.html]**:
- Segment revenue **+25% YoY**; vendor **direct sales to cloud providers +94% YoY** (hyperscaler AI infrastructure push).
- Trailing-4-quarter vendor rank: **High End Routing & Aggregation: 1 Cisco, 2 Huawei, 3 Nokia**; **Cloud Provider segment: 1 Cisco, 2 HPE Juniper, 3 Nokia**; **Communication Service Provider segment: 1 Huawei, 2 Cisco, 3 Nokia**.

**Fortune Business Insights (enterprise networking)**: Huawei listed among top vendors with ~18% enterprise networking market share (vs Cisco 22%) **[secondary — https://www.fortunebusinessinsights.com/enterprise-networking-market-105887]**.

### 1.5 International market status

- Huawei's networking growth remains anchored in **China and select emerging markets** (Middle East, Africa, Latin America, Southeast Asia) — per IDC's characterization of its router business **[independent]**.
- Export-control environment: Huawei continues to operate its networking business under US entity-list restrictions (imposed 2019); no lifting or change in 2026 found in sources fetched — treat current status as **[unverified]** for any specific 2026 regulatory change.
- No 2026 Huawei telecom/datacom wins in US, EU-5, Japan, Australia, or UK markets were found in fetched sources — consistent with the ongoing restrictions **[secondary]**.

---

## 2. MIKROTIK — 2026 product launches

### 2.1 MWC Barcelona 2026 launches (March 2026)

MikroTik announced a batch of new products at Mobile World Congress, Barcelona, March 2026 **[secondary — https://www.youtube.com/watch?v=KATjJn3RYUk; official newsletter — https://box.mikrotik.com/d/b5b340fd548c40fe97a3/files/?p=%2Fnews_132.pdf&dl=1]**:

**hEX Pro / hEX Pro PoE** (product codes **RB660UG+S+Me** / **RB660UPG+S+Me**) — next-generation SMB routers:
- CPU: **quad-core ARM Cortex-A73 @ 2.2 GHz**, ARM 64-bit; **2 GB DDR4** RAM; 128 MB NAND storage **[official — https://box.mikrotik.com/seafhttp/files/a87a30d3-5b64-46ce-b3fb-bbab549f71bd/hEX%20PRO%3AhEX%20PRO%20POE.pdf]**.
- Ports: **4× 2.5G Ethernet + 1× 10G Ethernet + 1× 10G SFP+**; **1× M.2 slot** for container storage (Docker-compatible containers); 1× USB 3.0 type A; switch chip **IPQ-9570**; RouterOS v7, license level 4.
- hEX Pro PoE adds **PoE-Out 802.3af/at** on Ether2–Ether5 (max 147 W total) and **MACSec 802.1AE** support on Ether1–Ether5.
- Powering: 24–57 V (DC jack + PoE-In 802.3bt); operating temperature **-40°C to +70°C**; max power 16 W (non-PoE) / 147 W (PoE).
- Positioning: 10G-uplink SMB router with on-device containers — a step up from the classic hEX (E50UG refresh, $59.95 MSRP, 880 MHz dual-core / 256 MB RAM) **[official/secondary — https://multilink.us/shop-by-brand/mikrotik/mikrotik-routers/]**.

**nRAY gen2** — 60 GHz wireless link reaching **up to 10 km** with 5 GHz backup link **[secondary]**.

**mAP ax** — pocket-size **travel router with Wi-Fi 6** **[secondary]**.

**MikroTik Scout** — **mesh communication device that works without internet coverage**; positioned for agriculture and rural areas **[secondary]**.

**A42GO-HbeP** — **Wi-Fi 7 access point with integrated GPON ONT in a single device**; described as the flagship product for ISPs (combines fiber termination + Wi-Fi 7 in one box) **[secondary]**.

### 2.2 Ampere Altra CCR family (Newsletter #132, April 2026)

MikroTik's April 2026 newsletter (#132) announced its **most powerful Cloud Core Router family to date, powered by Ampere Altra ARM server CPUs** — up to **64 CPU cores** and **64 GB DDR4**, aimed at core/aggregation roles in regional data centers, enterprise/campus networks, government/financial infrastructure, and telecom **[official — https://box.mikrotik.com/d/b5b340fd548c40fe97a3/files/?p=%2Fnews_132.pdf&dl=1]**.

**CCR3232-16XG-4DS-2DQ** (first model):
- **2× 200G QSFP56 + 4× 50G SFP56 + 16× 10G Ethernet** ports; **Marvell DX7335 switch chip with L3 offload**; **MACSec 802.1AE hardware encryption on all ports**; full internet routing tables, BGP, MPLS, advanced filtering; container support; RouterOS v7 **[secondary — https://www.store.mikrotikcanada.ca/ethernet-routers/750-ccr3232-16xg-4ds-2dq-4752224000026.html]**.
- Retail listing: **~$2,795** (Sil Micro, "COMING SOON", in stock per listing page) **[secondary — https://www.silmicro.com/cloud-core-routers/]** — price not confirmed by MikroTik; treat as [unverified].
- Context: community had spotted RouterOS builds for "AMPERE" on MikroTik's download page as early as Nov 2023 (forum thread), indicating a long testing cycle **[secondary — https://forum.mikrotik.com/t/mikrotik-ampere-cpu-coming-soon/171188]**.

### 2.3 ROSE Data Server RDS2216

"High-performance, all-in-one storage, networking, and container platform for enterprise environments" running **ROSE (RouterOS Edition for storage/compute)** **[secondary — https://www.techradar.com/pro/this-is-amazons-first-foray-in-servers-and-certainly-not-the-last-microtik-franken-router-is-powered-by-the-aws-graviton-1-arm-cpu]**:
- **16-core AWS Graviton 1 ARM CPU @ 2 GHz**, **32 GB DDR4**; **16 ports**: 2× 100G QSFP28, 4× 25G SFP28, 4× 10G SFP+, 2× 10G Ethernet; **20× U.2 NVMe storage slots**; NVMe-TCP block device export, encryption layers; USB ports.
- Container-ready: runs MinIO, Nextcloud, Shinobi, Frigate and other OCI-compliant containers; **no subscriptions or paywalls** (MikroTik's standard model).
- Distinctive green 1U chassis.

### 2.4 CRS switch line: 2026 pricing snapshots

- **CRS310-8G+2S+IN**: 8× 2.5G Ethernet + 2× SFP+; ARM 32-bit 98DX226S @ 800 MHz; RouterOS v7 license L5; ~**R4,090** (South Africa, Sep 22, 2026) **[secondary — https://www.comx-computers.co.za/MT-RBCRS310-8G-2S-IN-MikroTik-Cloud-Router-Switch-8x-Buy-p-290412.php]**.
- **CRS304-4XG-IN**: 4× 10G Ethernet (no modules needed) + 1× 1G management; fanless; ~**NZD 368.70** **[secondary — https://www.pbtech.com/pacific/product/NETMKT1502/MikroTik-Cloud-Router-Switch-CRS304-4XG-IN-with-4]**.
- **CRS317-1G-16S+RM**: 16× SFP+ + 1G management, dual PSU; ~NZD 871.30 **[secondary — https://www.pbtech.com/pacific/product/NETMKT1248/MikroTik-CRS317-1G-16SRM-Cloud-Router-Switch-CRS31]**.
- **CRS354-48G-4S+2Q+RM**: 48× 1G + 4× 10G SFP+ + 2× 40G QSFP+; 336 Gbps switching capacity, 235 Mpps; dual redundant PSU **[secondary — https://www.pbtech.com/product/NETMKT12401/MikroTik-CRS354-48G-4S2QRM-Cloud-Router-48-Port-Gi]**.
- MikroTik's pricing model remains: hardware price includes RouterOS license + **free software updates for the life of the product (minimum 5 years)**; no subscriptions **[official]**.

### 2.5 RouterOS v7 — 2026 release timeline

As of September 22, 2026 **[secondary — https://endoflife.ai/article-mikrotik-routeros-cve-2026-86060-only-6-49-21-fixed]**:

| Line | Channel | First release | Status |
|---|---|---|---|
| 7.21 | Long-term (previous) | Jan 12, 2026 | Last build 7.21.5 (Jul 6, 2026); in vulnerable range for Sep CVEs |
| 7.22 | Stable (superseded) | Mar 10, 2026 | Superseded by 7.23; in vulnerable range |
| 7.23 | **Long-term** | May 26, 2026 | Current LTS; 7.23.4, 7.23.5 (Sep 4), 7.23.6, 7.23.7 (Sep 16) |
| 7.24 | **Stable** | Aug 17, 2026 | Current stable; 7.24.2, 7.24.3, 7.24.4 |
| 7.25 | Beta | — | 7.25beta3 (Sep 3, 2026) |
| 6.49 | 6.x stable/long-term | Oct 6, 2021 | 6.49.21 (Sep 3, 2026) — last maintained 6.x line |
| 6.48 and earlier | None | — | End-of-life; no fixes |

### 2.6 September 2026 security incident ("MikroTrick")

- **Sep 3, 2026**: MikroTik published patched releases across all active channels (**6.49.21, 7.23.4, 7.24.2, 7.25beta3**); CERT Polska confirmed the releases block observed attacks **[secondary — https://startupfortune.com/mikrotik-routers-are-being-hijacked-through-a-flaw-patched-days-ago/; https://www.penligent.ai/hackinglabs/cve-2026-67277/]**.
- **CVE-2026-67277**: RouterOS memory disclosure + remote DoS via the bandwidth-test service; listed on **CISA KEV** as known-exploited **[secondary]**.
- Exploitation observed in the wild from **~Sep 2, 2026** — effectively a 0day window (fixes released Sep 3); campaign dubbed **"MikroTrick"** by community/press **[secondary — https://medium.com/@costin.raiu/mikrotik-ssh-0day-exploitation-in-the-wild-20da4587d9b2]**.
- New **"Flagged" self-check**: patched builds detect known tampering artifacts at boot, disable suspicious entries, log a critical message, and mark the device as Flagged **[secondary — https://dev.to/bianliang/how-many-mikrotik-devices-are-actually-reachable-from-the-internet-1h76]**.
- Exposure: third-party estimate of **~122,000 exposed and potentially vulnerable instances** (ZoomEye/Shodan-based measurement, Sep 19, 2026 — population measurement, not confirmed compromises) **[secondary]**.
- Related issue: **7.23.6 / 7.24.3 deleted modem firmware on LTE devices** (RBSXTLTE3-7, EC25-EU, EG25-G variants); fixed in 7.23.7 / 7.24.4 (Sep 16), with manual modem firmware upgrade required **[secondary — https://blog.j2sw.com/netops/mikrotik-7-23-7-24-and-lte/]**.
- MikroTik guidance: do not expose SSH/WebFig/bandwidth-test to untrusted networks; use VPN (e.g., WireGuard) for management **[secondary]**.

---

## 3. NETWORK SWITCH PRICING TRENDS 2026 (25G / 100G / 400G)

### 3.1 Market context

- Global Ethernet switch revenue **$18.9B in Q2 2026, +43.4% YoY** (IDC); **400G is the fastest-growing port-speed segment**; 100G remains the largest segment; 25G holds a significant share; 10G share is diminishing **[independent — https://www.idc.com/resource-center/blog/ethernet-switch-market-surges-43-4-to-18-9b-in-2q26-as-ai-infrastructure-demand-drives-record-datacenter-spending/]**.
- Dell'Oro (2022 5-year forecast, directionally): **400 Gbps and higher speeds forecast to comprise half of data-center switch spending**; 800 Gbps adoption led by Google/Meta/Microsoft **[independent — https://www.lightreading.com/it-infrastructure/dell-oro-data-center-switch-market-to-approach-100b-over-the-next-five-years]**.

### 3.2 Switch hardware price points (marketplace snapshots, Sep 2026 — secondary)

| Switch | Ports | Price (marketplace) | Per-port approx |
|---|---|---|---|
| Juniper QFX5220-32CD | 32× 400G QSFP-DD | ~$5,000 | ~$156 |
| Dell PowerSwitch Z9664F-ON | 64× 400G QSFP56-DD | ~$24,448–24,947 | ~$382–390 |
| NVIDIA SN4700 (Spectrum-3) | 32× 400G QSFP-DD | ~$34,638 | ~$1,082 |
| Arista DCS-7280CR3MK-32D4S | 32× 100G + 4× 400G | ~$15,000 | mixed |
| Huawei CE6881-48S6CQ-F | 48× 25G + 6× 100G | ~$2,630–2,638 | ~$49 (25G ports) |

⚠️ **Caveat:** these are third-party marketplace listings (Accio, HardwareNation), not vendor MSRP; grey-market/refurb units distort prices; treat as indicative only **[secondary]**.

### 3.3 Optics pricing (2026)

- **400G QSFP-DD optics**: SR **$1,400–2,500**; LR **$2,500–4,000** (vs 100G QSFP28 SR $200–500, LR $400–800); coherent ZR $8,000–15,000 **[secondary — https://www.fibermall.com/blog/qsfp-dd-vs-qsfp28-vs-osfp-comparison.htm]**.
- Data-center segment typical 400G module unit price: **$850–1,100** (Dataintelo market research) **[secondary — https://dataintelo.com/report/400g-qsfp-dd-optical-module-market]**.
- QSFP-DD switches average **$200–400 less per port** than OSFP equivalents (higher volume, simpler thermal design) **[secondary]**.
- **IP transit pricing (Telegeography, Q2 2026)**: 100 GigE port prices fell **11–13% compounded annually (2023–2026)** in London/NYC/Singapore; weighted median **$0.08/Mbps** in US/EU; 400G/100G port MRC multiple averaged **3.5**; 400 GigE sales growing fast on hyperscaler/AI demand; 400G adoption still limited in Asia/LatAm/Africa **[independent — https://resources.telegeography.com/ip-transit-price-erosion-significant-regional-differences-remain]**.

---

## 4. OPEN-SOURCE NETWORKING OS — 2026 BRIEF

### 4.1 SONiC (the headline story)

- **Cisco opening SONiC to all customers (May 2026)**: Cisco announced plans to let any customer run the hardened open-source SONiC NOS on its **Nexus 9000 series data-center switches** — previously limited to hyperscalers. Platforms built on Cisco Cloud Scale / Silicon One, **alongside NVIDIA Spectrum-X Ethernet switch silicon for AI-class fabrics**; hardened code, TAC backing, Nexus Dashboard integration; customers can run SONiC for AI workloads while keeping ACI/NX-OS on the same boxes **[independent — https://www.theregister.com/networks/2026/05/27/cisco-making-sonic-available-to-all-customers-not-just-hyperscalers/5246733; secondary — https://www.webpronews.com/cisco-opens-sonic-to-enterprise-networks-as-ai-demands-push-beyond-hyperscalers/]**. Driver: enterprises building large on-prem AI clusters want hyperscaler-style fabrics without public cloud.
- **SONiC Foundation (Aug 2026)**: welcomed **Supranett as a Premier Member** (AI infrastructure builder) plus Exaware, TeraHop, Infrawaves as General Members — reflecting production SONiC adoption across hyperscale clouds, enterprises, telcos, and AI data centers **[secondary — https://www.prnewswire.com/news-releases/sonic-foundation-welcomes-supranett-as-a-premier-member-to-advance-open-networking-for-ai-infrastructure-302836236.html]**.
- Market scale: Gartner estimates **<5% of the world's 100,000+ data centers run SONiC in production** (~100–200 large-scale enterprise deployments), but SONiC-based DC switching revenue projected **>$5B by 2026, >$8B by 2027**, potentially **>$15B by 2030** (>25% CAGR); adopters include LinkedIn, eBay, Tencent, Verizon, Orange **[secondary — https://plvision.eu/blog/opensource/sonic-nos-for-private-data-centers]**.

### 4.2 FRRouting (FRR)

- Active maintenance in 2026: **frr-10.2.x** patch releases (10.2.5 → 10.2.6) with BGP/IS-IS/OSPF hardening backports **[secondary — https://github.com/FRRouting/frr/releases]**.
- **Ubuntu USN-8046-1 (Feb 17, 2026)**: multiple FRR security fixes — malformed OSPF/BGP update packets could crash daemons (DoS); CVEs **CVE-2025-61099 through CVE-2025-61107**; fixed in frr 10.4.1 (Ubuntu 25.10), 8.4.4 (Ubuntu 24.04 LTS), 8.1 (Ubuntu 22.04 LTS) **[independent — https://ubuntu.com/security/notices/USN-8046-1]**.
- FRR remains the default routing stack in netlab/containerlab topologies and whitebox NOS builds (SR-MPLS for OSPFv2 added in 2026 tooling) **[secondary — https://github.com/ipspace/netlab/blob/HEAD/docs/release/26.03.md]**.

### 4.3 DANOS / OpenSwitch

- **DANOS**: the DANOS-Vyatta edition (AT&T's open-source NOS, Linux Foundation) is positioned as a **cell site gateway router (CSGR)** solution for 5G RAN backhaul transport; **no specific 2026 release or project milestone was located** in fetched sources — the project appears quiet in 2026 **[secondary — https://slashdot.org/software/comparison/Cisco-IOS-XE-vs-DANOS-Vyatta/; unverified status]**.
- **OpenSwitch (OPX)**: **no 2026 news located** — last notable upstream activity predates the window; treat the project as dormant/stale for RAG purposes **[unverified]**.
- Related: NVIDIA's **Cumulus Linux** continues as the commercial open NOS reference on Spectrum switches (e.g., SN4700 ships with Cumulus Linux / ONIE) **[secondary]**.

---

## 5. OPEN VERIFICATION LOG

1. **MikroTik CCR3232-16XG-4DS-2DQ price** — ~$2,795 from a single retailer listing ("coming soon"); no official MSRP found. [unverified]
2. **MikroTik hEX Pro / hEX Pro PoE pricing and ship date** — specs from official MWC handout; no price or availability found. [unverified]
3. **MikroTik Scout / nRAY gen2 / mAP ax / A42GO-HbeP specs and pricing** — announced at MWC 2026 via video coverage only; official product pages not fetched. [unverified]
4. **MikroTik RDS2216 price and availability** — specs from TechRadar; no price found. [unverified]
5. **"MikroTrick" campaign attribution and scope** — ~122K exposed instances is a third-party population estimate, not confirmed compromises; attacker identity unknown. [secondary/unverified]
6. **CVE-2026-67277 technical details** — advisories are high-level; no vendor-published root-cause analysis located. [secondary]
7. **Huawei SF9300 / XH9300 specs** — all figures (30% cost, 40% latency/power, 100T/51.2T, 3.2T OE) are vendor-reported from a PRNewswire release; no independent testing. [vendor-reported]
8. **Huawei Stellar AI Fabric / NetMaster 95% fault-analysis claim** — vendor-reported; IHEP +10% efficiency is a customer quote via Huawei. [vendor-reported]
9. **Huawei international regulatory status 2026** — no change located in 2026; entity-list status assumed ongoing but not re-verified. [unverified]
10. **Switch/optics price table** — marketplace snapshots (Accio, HardwareNation), not vendor MSRP; grey-market distortion likely. [secondary]
11. **SONiC market revenue projections** ($5B 2026 → $15B 2030) — single secondary source citing Gartner; not cross-checked. [secondary]
12. **DANOS 2026 activity** — no releases found; "quiet" assessment is based on absence of evidence. [unverified]
13. **OpenSwitch 2026 activity** — no news found; "dormant" assessment is based on absence of evidence. [unverified]
14. **CloudMatrix 384 2026 deployments** — SemiAnalysis analysis is from Apr 2025; 2026 deployment scale (Ulanqab etc.) via secondary Chinese sources. [secondary]
15. **Fortune Business Insights Huawei 18% enterprise-networking share** — methodology unclear; conflicts in granularity with IDC figures. [secondary]

## 6. COLLECTION METADATA

- **Research date:** September 22, 2026.
- **File:** `~/workspace/rag_collect/etape6_trackC_huawei_mikrotik.md`, in English.
- **Source priorities used:** vendor press releases (Huawei, MikroTik newsletters/box), IDC, Dell'Oro, The Register, SemiAnalysis, Telegeography, Ubuntu security notices, community coverage (forums, YouTube, ISP press).
- **Conventions:** every factual claim carries a provenance tag; prices are dated snapshots (Sep 22, 2026); market shares are analyst-reported (IDC/Dell'Oro), not vendor claims; absence of evidence is flagged, not treated as evidence of absence.
- **No consolidation applied** — research delivered as-is per project rule (Sep 22, 2026).
