# Step 6 — Track D: FortiGate + pfSense + OPNsense (Firewalls & Network Security)

**Project:** RAG data-collection, Step 6 (Réseau & sécurité)
**Coverage window:** February 1, 2026 → September 22, 2026
**Status:** Research snapshot. Prices and version figures are dated snapshots as of September 22, 2026.

### Provenance legend
- **[official]** — vendor's own blog, docs, release notes, earnings statement, or advisory.
- **[vendor-reported]** — figure claimed by the vendor (benchmarks, market share) without independent audit.
- **[independent]** — reputable third-party press (BleepingComputer, The Register, Dark Reading, Reuters, IDC, Gartner) or independent measurement.
- **[secondary]** — lower-tier press, blogs, analyst summaries, community trackers; useful but unverified.
- **[unverified]** — single-source or conflicting claims; treat as uncertain.

---

## 1. FORTINET / FortiGate

### 1.1 FortiOS 8.0 — the headline OS release (announced March 10, 2026)

- Fortinet launched **FortiOS 8.0** at its **Accelerate 2026 conference in Las Vegas** on **March 10, 2026** — the new OS release powering the Fortinet Security Fabric **[secondary — https://www.stocktitan.net/news/FTNT/fortinet-introduces-forti-os-8-0-to-expand-secure-networking-with-iv0y66jrnxym.html; https://www.coffeefranchisehub.com/archives/42978]**.
- FortiOS 8.0 groups its updates into three pillars: **AI-driven security, next-generation SASE, and quantum-safe protection** **[secondary — https://securitybrief.co.uk/story/fortinet-s-fortios-8-0-adds-ai-quantum-safe-tools]**.
- **AI usage controls / governance** (new in 8.0):
  - **FortiView for AI**: attack-surface and shadow-AI visibility — shows how AI applications/services are used across the organization, distinguishing sanctioned vs unsanctioned tools **[secondary]**.
  - **AI-aware application control** for generative-AI usage; visibility into **Model Context Protocol (MCP) and agent-to-agent (A2A) interactions** to reduce blind spots **[secondary]**.
  - Enhanced data loss prevention (DLP) powered by **optical character recognition (OCR)** **[secondary]**.
  - AI agents across the Fortinet Security Fabric enabling conversational troubleshooting and configuration in firewall and SD-WAN environments **[secondary]**.
- **SASE in 8.0**: **FortiSASE Outpost** and sovereign deployment options for data residency; unified SD-WAN bundles; multipath IPsec tunnels **[secondary]**.
- **Quantum-safe**: quantum-resilient cryptographic controls, hybrid post-quantum cryptography (PQC) SSL deep inspection **[secondary]**.
- **ZTNA in 8.0** (from the official FortiOS 8.0 release notes): a FortiGate can now act as a **ZTNA service connector**, reverse-proxying service connections upstream to the ZTNA Edge (e.g., FortiPAM or FortiProxy acting as ZTNA access proxy); configuration simplification/modularization (`firewall.access-proxy` split into `ztna.traffic-forward-proxy`, `ztna.web-proxy`, `ztna.web-portal`; new `ztna.destination` object); IPv6 security posture tags/groups for dual-stack ZTNA policies; tag sharing with FortiClient EMS **[official — https://docs.fortinet.com/document/fortigate/8.0.0/fortios-release-notes/743723/new-features-or-enhancements]**.
- Ken Xie quote (CEO): *"FortiOS 8.0 reflects more than 25 years of continued innovation at the intersection of networking and security. As organizations embrace AI, cloud, and increasingly encrypted environments, a unified operating system is essential to reduce complexity, improve visibility, and ensure security can scale without slowing the business."* **[secondary]**

### 1.2 New FortiGate hardware in 2026 (G series)

**FortiGate 3500G + FortiGate 400G — announced May 6, 2026**
- New additions to the **FortiGate G series**, positioned to secure AI workloads and encrypted traffic in data centers and enterprise edges **[official/via GlobeNewswire — https://www.globenewswire.com/news-release/2026/05/06/3288892/0/en/Fortinet-Expands-FortiGate-G-Series-to-Secure-AI-from-the-Data-Center-to-Modern-Enterprise-Edges.html]**.
- Powered by Fortinet's **NP7 and SP5** custom ASICs and FortiOS **[official]**.

**FortiGate 1200G — announced July 28, 2026**
- Midrange G-series appliance: **10G, 25G and 100G connectivity**; **397 Gbps firewall throughput**; aimed at campus, data center and hybrid environments inspecting large volumes of encrypted traffic **[secondary — https://www.csoonline.com/article/4202566/fortinets-new-fortigate-platform-converges-firewall-sase-technologies-2.html; https://www.itp.net/cybersecurity/fortinet-launches-fortigate-1200g-to-advance-firewall-and-sase-convergence]**.
- Fills the gap between campus/edge/branch appliances (e.g., 400G) and flagship data-center platforms (e.g., 3500G) **[secondary]**.
- Ships with **FortiOS 8.0** and can operate as a **FortiSASE Outpost**: the 1200G becomes a local SASE point of presence (POP) inside customer-controlled environments (on-prem sites, private DCs, colocation), enforcing security locally while staying centrally managed from the cloud — targeting data-sovereignty and latency-sensitive workloads **[secondary]**.
- **Availability expected in Q3 2026** **[secondary]**.
- Integrates **FortiGuard AI-Powered Security Services** and **FortiAI** for real-time threat intelligence and AI-assisted SecOps **[secondary]**.

### 1.3 Intel SP6 collaboration (July 29, 2026)

- Fortinet announced a **strategic collaboration with Intel to develop Fortinet Security Processor 6 (SP6)**, combining Fortinet's proprietary security-processor expertise with Intel's design, development, packaging and manufacturing capabilities, strengthening supply-chain resilience and diversity **[official — via Q2 2026 earnings, https://www.globenewswire.com/news-release/2026/07/29/3335555/0/en/fortinet-reports-strong-second-quarter-2026-financial-results.html]**.
- Secondary reporting (TradingView/UDIS): Intel Foundry will co-develop and manufacture the SP6 ASIC; Fortinet is reportedly **the first external cybersecurity customer for Intel Foundry and the first external customer on the Intel 4 node**, with production at **Fab 34 in Ireland**; the SP6 moves from the monolithic 7nm design of the SP5 to a **disaggregated chiplet architecture** for better yields and lower unit costs **[secondary — https://www.tradingview.com/chart/FTNT/vj3CqrYb-Why-Is-Intel-Building-Fortinet-s-Next-Firewall-Chip/; unverified in part — Intel Foundry customer specifics not independently confirmed in this research]**.

### 1.4 Financials — Q2 2026 results (July 29, 2026)

- Revenue **$2.05B (+26% YoY)**; product revenue **$773M (+52% YoY)**; billings **$2.37B (+33% YoY)**; GAAP operating margin 34%; non-GAAP operating margin **38%**; GAAP EPS $0.82 (+44%); non-GAAP EPS $0.90 (+41%); operating cash flow $1.04B; free cash flow $966M **[official — https://www.globenewswire.com/news-release/2026/07/29/3335555/0/en/fortinet-reports-strong-second-quarter-2026-financial-results.html]**.
- FY2026 revenue guidance raised to **19% YoY growth**; Q3 2026 EPS guidance $0.83–0.87 (non-GAAP); FY2026 EPS guidance $3.41–3.47 **[official/secondary — https://www.americanbankingnews.com/2026/09/17/newedge-advisors-llc-increases-holdings-in-fortinet-inc-ftnt.html]**.
- FY2025 context: total revenue **$6.80B**, net income $1.85B, free cash flow $2.21B **[secondary — StockTitan 10-K reference]**.
- CEO Ken Xie framing: customers value Fortinet's **"SASE Firewall"** — firewall, SD-WAN and SASE integrated on a single FortiOS, powered by FortiASIC, with sovereign deployment options **[official]**.

### 1.5 Market position 2026

- Fortinet is the leader in firewall **unit shipments**: **over 50% share by unit shipments** (Beaver Research, Jan 2026, citing IDC) — one secondary source cites ~48% **[secondary — https://medium.com/@beaverresearch/discounted-firewall-king-fortinets-high-quality-growth-at-a-bargain-0bf76c98d817; https://www.tradingview.com/chart/FTNT/vj3CqrYb-Why-Is-Intel-Building-Fortinet-s-Next-Firewall-Chip/]**.
- **750,000+ customers** worldwide; ~80% of the Fortune 100 and 72% of the Global 2000 use Fortinet products **[secondary, vendor-cited]**.
- Named a **Leader in the 2026 Gartner Magic Quadrant for Hybrid Mesh Firewall** **[secondary — via MarketBeat headline roundup, https://www.marketbeat.com/instant-alerts/filing-fortinet-inc-ftnt-shares-bought-by-newedge-advisors-llc-2026-09-17/]**.
- Secure Networking addressable market estimated by Fortinet at ~$86B by 2027 (~9% annual growth); Universal SASE ~$36B by 2027 (~20% growth) — Fortinet's own market sizing, [vendor-reported] **[secondary — https://www.stocktitan.net/news/FTNT/fortinet-sharpens-business-focus-on-core-growth-areas-to-extend-5ukuetb3r4or.html]**.

### 1.6 Vulnerabilities and advisories 2026

**CVE-2026-24858 — FortiCloud SSO improper cryptographic signature verification (zero-day, actively exploited)**
- Disclosed **late January 2026** (Fortinet CISO blog post; patched FortiOS **7.4.11**, with 7.6.6 and 8.0.0 follow-ups) **[independent — https://www.cybersecurity-help.cz/blog/5202.html]**.
- Exploitation observed **starting January 15, 2026** (Arctic Wolf); attackers created new local admin accounts, granted VPN access, and exfiltrated firewall configurations even on fully patched devices; two malicious FortiCloud accounts disabled on **January 22, 2026** **[independent — https://www.inforisktoday.com/fortinet-locks-down-forticloud-sso-amid-zero-day-attacks-a-30612]**.
- Root cause: an attacker with a FortiCloud account and a registered device could log into **other devices registered under different accounts** when FortiCloud SSO was enabled. Fortinet clarified it impacts **FortiCloud SSO only** (not third-party SAML IdP or FortiAuthenticator) **[independent — https://Www.Darkreading.com/vulnerabilities-threats/fortinet-new-zero-day-malicious-sso-logins]**.
- Initially suspected to be a bypass of the December 2025 patch for **CVE-2025-59718** (FortiCloud SSO authentication bypass via crafted SAML messages; Arctic Wolf reported active exploitation in December 2025; added to CISA KEV), plus companion **CVE-2025-59719**. Fortinet confirmed CVE-2026-24858 is a **separate, new vulnerability**, not a patch bypass **[independent — https://www.inforisktoday.com/fortinet-locks-down-forticloud-sso-amid-zero-day-attacks-a-30612; https://www.techradar.com/pro/security/fortinet-fortigate-devices-hit-in-automated-attacks-which-create-rogue-accounts-and-steal-firewall-data]**.
- CVE-2026-24858 was added to the **CISA Known Exploited Vulnerabilities (KEV)** catalog **[independent — Dark Reading]**.
- Workaround while unpatched: disable FortiCloud SSO admin login (`set admin-forticloud-sso-login disable`) **[independent — TechRadar via BleepingComputer]**.

**"FortiBleed" credential-compromise reporting (unverified as a zero-day)**
- An EOTISEC analytical report (EOTISEC-2026-044, June 2026) drew on findings by researcher Volodymyr Diachenko, Kevin Beaumont, Hudson Rock and SOCRadar describing a large validated credential set tied to Fortinet devices. The report explicitly states **no Fortinet zero-day was confirmed as of its issue date** **[secondary — https://sveoti.net/wp-content/uploads/2026/06/EOTISEC-2026-044_FortiBleed.pdf]**. Treat the credential set as [unverified]; treat "no confirmed zero-day" as the report's own assessment **[unverified]**.
- Separately, a September 2026 underground forum post claimed a **private FortiGate RCE 1-day for sale**; analysis (UNDERCODE NEWS) concluded it is an exploit-sale claim with **no CVE, root cause, affected build range, or independent reproduction** — not evidence of a new zero-day **[secondary — https://undercodenews.com/fortigate-under-pressure-underground-1-day-rce-sale-raises-fresh-alarm-for-enterprise-vpn-security-video/; unverified]**.

### 1.7 FortiAI / AI security features (context)

- FortiGuard AI-Powered Security Services and **FortiAI** integrated into the FortiGate 1200G for real-time threat intelligence, automated protection and AI-assisted SecOps **[secondary — ITP.net]**.
- Prior 2026-adjacent AI launches cited by StockTitan Argus: **Secure AI Data Center blueprint with Arista** deployed at MPS; **FortiGate VM integrated on NVIDIA BlueField-3 DPUs** for AI workloads; FortiGate 3800G for Secure AI Data Center; AI-powered workspace/email security suite (Jun 4, 2026) **[secondary]**.
- FortiOS 8.0 AI agents enable conversational troubleshooting and configuration across firewall and SD-WAN **[secondary]**.

---

## 2. pfSense (Netgate)

### 2.1 Release timeline 2026 (pfSense Plus, all on FreeBSD 16.0-CURRENT)

| Version | Released | Base OS | Config Rev | Notes |
|---|---|---|---|---|
| 25.11.1 | 2026-01-26 | FreeBSD 16.0-CURRENT | 24.2 | Jan 2026 patch release |
| 26.03 | 2026-04-01 | FreeBSD 16.0-CURRENT | 24.5 | Major 2026 release |
| 26.03.1 | 2026-05-27 | FreeBSD 16.0-CURRENT | 24.5 | Patch (IPv6 NDP regression reported) |
| 26.07 | 2026-08-13 | FreeBSD 16.0-CURRENT | 24.6 | Nexus controller era begins |
| 26.10 | TBD | FreeBSD 16.0-CURRENT | 24.6 | Not yet released as of Sep 22, 2026 |

**[official — https://docs.netgate.com/pfsense/en/latest/releases/versions.html]**

- **pfSense Plus 26.07 (August 13, 2026)** is the headline 2026 release. It advances the **Netgate Nexus controller** — a new **Go-based controller replacing the legacy PHP GUI** and serving as the modern foundation for pfSense Plus **[official — https://forum.netgate.com/topic/201119/netgate-releases-pfsense-plus-software-version-26.07]**.
- 26.07 exclusive Nexus features: **CoreDNS** (high-performance integrated DNS, via the new Netgate `rexdns` plugin), **Threatgate** (bulk address/domain lists for firewall rules, aliases and CoreDNS groups), and **Snort 3** (multi-threading support, faster rule syntax) available only via the Nexus GUI **[official]**.
- 26.07 security: **critical WireGuard fix for CVE-2026-58085** plus other security enhancements **[official]**.
- Other 26.07 fixes: DHCP, DNS Resolver, DynamicDNS, gateways/monitoring, IPsec, VXLAN interfaces, OpenVPN, firewall rules/NAT, traffic shaper, wireless **[official]**.
- **pfSense CE 2.9.0** exists on **FreeBSD 16.0-CURRENT** (pfREST 2.10) — reported via community/GitHub compatibility matrices, not the main Netgate release notes; treat as **[secondary]**.
- Netgate policy: new pfSense releases track **FreeBSD-CURRENT**, not RELEASE — official blog *"pfSense software is moving ahead"* confirmed they will stay on CURRENT with no return to RELEASE **[secondary — https://forum.netgate.com/topic/199339/latest-pfsense-release-25-11-uses-freebsd-16-official-release-is-december-2027]**.
- Known issue: **IPv6 NDP regression on pfSense Plus 26.03.1** (Redmine #16919, Jun 30, 2026) **[official — Redmine, https://redmine.pfsense.org/projects/pfsense-plus/issues.pdf]**.

### 2.2 Pricing and licensing 2026

- **pfSense Plus software (3rd-party hardware, Netgate Installer): starting at $129/yr** — no feature or throughput upcharges **[official — https://www.netgate.com/pricing-pfsense-plus]**.
- Cloud (AWS/Azure marketplaces): **$0.08–$0.40/hr** for the pfSense Plus software component; free 30-day trials **[official]**.
- Netgate appliances include pfSense Plus at no charge; appliances start at **$189** (e.g., Netgate 1100 at $269 with TAC Lite) **[official]**.
- TAC support: **TAC Pro $399/yr**, **TAC Enterprise $799/yr** (Netgate 1100 options: TAC Pro 1-yr $49 / 2-yr $98 as appliance add-ons) **[official/secondary — https://aws.amazon.com/marketplace/pp/prodview-lstvktrnia26i; https://shop.netgate.com/products/1100-pfsense]**.
- **Multi-instance management (MIM) entitlements**: Netgate Nexus multi-instance management is sold **per managed device**; the controller can manage the instance it resides on by default; additional entitlements expire/renew **yearly** — this generated community backlash in 2026 about the end of the "lifetime free" 2022 Home+Lab promotion terms **[official via forum/FAQ + community — https://forum.netgate.com/topic/201119/netgate-releases-pfsense-plus-software-version-26.07; https://forum.netgate.com/topic/201061/a-new-release-candidate-for-pfsense-plus-software-version-26.07-available/18]**.
- Context: the free Home+Lab download was discontinued years earlier (abuse by third-party vendors); CE remains fully free **[secondary — https://techunwrapped.com/netgate-reverses-course-and-pfsense-plus-homelab-will-never-be-free-again/]**.

### 2.3 Netgate company/product news 2026

- No new flagship appliance model launches were verified in this research window (Netgate 1100/2100/4100/6100/8200 lineup continues; 6100 HA deployments discussed in 26.07 forum threads) **[secondary/unverified]**.
- Nexus is the strategic direction: Netgate is rebuilding management around the Go-based Nexus controller, with MIM (multi-instance management) as the monetized management plane **[official — forum announcements]**.

---

## 3. OPNsense (Deciso)

### 3.1 Release model

- OPNsense follows a **YY.M cadence** with a codename per major: majors in **January and July** each year, with bi-weekly-ish minor patches. Major upgrades are sequential (no skipping) **[secondary — https://github.com/lindolfojunior/skills-evolver/blob/HEAD/skills/opnsense-firewall/SKILL.md]**.
- License: clear and stable **2-Clause BSD** **[official — https://docs.opnsense.org/releases/CE_26.7.html]**.

### 3.2 OPNsense CE 26.1 "Witty Woodpecker" (released ~January 28, 2026)

- Released late January 2026; upgrade path from 25.7 unlocked **January 29, 2026** **[official — https://docs.opnsense.org/releases/CE_26.1.html]**.
- Headline features: almost a **full firewall MVC/API experience** (automation rules promoted to the new rules GUI); **Suricata 8 with inline inspection mode using "divert"**; IPv6 reliability/feature improvements; **router advertisements MVC/API**; full code shell-command escaping revamp; **default IPv6 mode now using Dnsmasq** for client connectivity; Unbound blocklist source selection; **automatic host discovery service** **[official]**.
- Patch series through July 2026: **26.1.10 (June 15, 2026)** — 3 core security issues, FreeBSD security advisories, source-NAT migration prep, FreeBSD 15.1 support groundwork, MVC rules GUI fixes; **26.1.11 (July 1, 2026)** — security advisories incl. config-line injection via GUI text fields, stored XSS in GPS init string, OpenVPN path traversal in common_name, plus FreeBSD security advisories (VM use-after-free, execve TOCTOU, OpenZFS, KTLS, TCP RACK, POSIX shm, audit, iconv, libalias); hotfixes **26.1.11_3 / _5 / _6 / _10** through early July **[official — https://docs.opnsense.org/releases/CE_26.1.html]**.

### 3.3 OPNsense CE 26.7 "Xenial Xenops" (released July 15, 2026)

- Released **July 15, 2026** — *"Building the foundation for the next generation of open network security"* (Deciso PR via PR Newswire) **[official/secondary — https://docs.opnsense.org/releases/CE_26.7.html; https://tools.prnewswire.com:443/en-us/live/19304/release/20260715UN05123?filter=19304]**.
- Headline changes:
  - **Interface assignments and gateway groups via MVC/API** — first implementation of the new Interface Assignments framework; establishes the foundation for future **API-driven interface management** (per Deciso CIO Ad Schellevis) **[official — https://linuxiac.com/opnsense-26-7-open-source-firewall-released-powered-by-freebsd-15-1/]**.
  - **Firewall rules now default to MVC/API**; **outbound NAT → source NAT migration assistant** **[official]**.
  - **Captive portal IPv6 support**; **Kea DDNS, custom options, dynamic prefix delegation** (dynamic prefix delegation distributes changing IPv6 prefixes downstream) **[official — https://linuxiac.com/opnsense-26-7-open-source-firewall-released-powered-by-freebsd-15-1/]**.
  - Under the hood: **FreeBSD 15.1**, **OpenSSL 3.5**, **OpenVPN 2.7**, **PHP 8.5**, **Python 3.13** **[official]**.
  - Security hardening: multiple **stored XSS fixes**, shell-safe escaping, legacy exec() removal continuing **[official]**.
- Hotfix **26.7.1_1** issued: default-gateway-switch alarm trigger fix, pecl-mcrypt 1.0.9 **[official]**.
- 25.7-series maintenance continued in parallel: **25.7.11 (January 15, 2026)** introduced the **host discovery service** (hostwatch 1.0.4, enabled by default, automatic MAC registry for IPv4/IPv6 feeding MAC-type aliases and captive portal) **[secondary — https://cybersecuritynews.com/opnsense-firewall-25-7-11-released/]**.

### 3.4 OPNsense Business Edition (Deciso commercial)

- **Business Edition 26.4 released April 17, 2026**, following Community Edition 26.1 "Witty Woodpecker" **[secondary — https://www.thefreelibrary.com/OPNsense+Business+Edition+26.4+Released.-a0883212425]**.
- BE 26.4 features: full MVC/API experience (automation rules in new rules GUI), Suricata divert inline inspection, IPv6 RA MVC/API, shell-escaping revamp, Dnsmasq default IPv6 mode, Unbound blocklist source selection, automatic host discovery, captive portal IPv6, **enhanced OpenVPN user portal with custom configurations**, **improved OpenID Connect** (identity-claim visibility; Microsoft Entra ID integration), **OPNcentral** opt-out for automatic login, **OPNWAF mod_status** diagnostics **[official — https://github.com/opnsense/docs/blob/HEAD/source/releases/BE_26.4.rst]**.
- **BE 26.4.1 (June 16, 2026)**: based on community 26.1.9 with reliability improvements **[official — https://github.com/opnsense/docs/blob/HEAD/source/releases/BE_26.4.rst]**.
- Prior BE 25.10 (Feb 9, 2026, 25.10.2): revamped frontend grid UI, experimental GUI privilege separation, firewall automation GUI, OIDC, captive portal backend rewrite, FreeBSD 14.3 **[official — https://github.com/opnsense/docs/blob/HEAD/source/releases/BE_25.10.rst]**.

### 3.5 Community and adoption notes

- The release-notes text notes "over 11 and a half years" of continuous development and credits a rising volume of core-code security reports from the community in mid-2026 (reporters acknowledged: **lujiefsi**, **Jonas Ampferl of Hacking Cult**, **Jan Kahmen of turningpoint**, **chrstnth**, **haxorton**, **Greelan**, **Konstantinos Spartalis**, **Etienne Girault**) — Deciso said it was improving its report-handling process after 26.7 **[official — https://docs.opnsense.org/releases/CE_26.1.html]**.
- No verified 2026 download/user-count figures were located in this research; Deciso positions OPNsense for home-to-enterprise use with the Community Edition free and the Business Edition as the commercial tier **[unverified — figures]**.

---

## 4. Firewall / network-security market 2026

- **NGFW market size 2026: ~$6.97B** (from ~$6.11B in 2025), projected to reach ~$13.52B by 2031 at ~14.15% CAGR (Research and Markets via press). Alternate estimates: $5.76B (Business Research Insights), $7.08B (Fortune Business Insights), $7.12B (Value Market Research, 12.46% CAGR to 2034). All agree on double-digit growth. **[secondary — https://www.researchandmarkets.com/reports/4602246/next-generation-firewall-market-share; https://www.businessresearchinsights.com/market-reports/next-generation-firewall-ngfw-market-119043; https://www.fortunebusinessinsights.com/next-generation-firewall-market-112358; https://www.valuemarketresearch.com/report/next-generation-firewall-market]**.
- Key players listed across reports: Palo Alto Networks, Fortinet, Check Point, Cisco, SonicWall, Juniper **[secondary]**.
- Trends: cloud-native/inline L7-aware security, **68% of enterprise workloads in public/private/hybrid clouds**; zero-trust network access embedded in NGFWs; AI-driven threat detection influencing ~73% of adoption (one report's figure, vendor-sourced); 800G and encrypted-traffic inspection driving appliance refresh **[secondary]**.
- Fortinet holds **>50% of firewall appliance unit shipments** (IDC, vendor-cited) — the standout vendor-share fact of 2026 **[secondary — https://medium.com/@beaverresearch/discounted-firewall-king-fortinets-high-quality-growth-at-a-bargain-0bf76c98d817]**.
- The firewall-refresh cycle that drove 2025–2026 demand was reported **40–50% complete as of August 2025** (management comment that caused a 22% one-day FTNT drop) — context for the H2 2025/H1 2026 hardware wave (G series) **[secondary]**.

---

## 5. Notable CVEs / advisories affecting these vendors in 2026

| CVE / advisory | Vendor | Description | Status |
|---|---|---|---|
| CVE-2026-24858 | Fortinet | FortiCloud SSO improper cryptographic signature verification; active exploitation Jan 15–22, 2026; rogue admin accounts + config exfiltration | Fixed in FortiOS 7.4.11, 7.6.6, 8.0.0; CISA KEV listed **[independent]** |
| CVE-2025-59718 / 59719 | Fortinet | Dec 2025 FortiCloud SSO SAML auth bypass; actively exploited Dec 2025 (Arctic Wolf) | Patched Dec 2025; CISA KEV listed **[independent]** |
| CVE-2026-58085 | pfSense/Netgate | WireGuard critical security update | Fixed in pfSense Plus 26.07 (Aug 13, 2026) **[official]** |
| FreeBSD security advisories (Jun–Jul 2026) | OPNsense/pfSense (upstream) | VM use-after-free, execve TOCTOU privesc, OpenZFS, KTLS, TCP RACK, POSIX shm, iconv, libalias RTSP overflow, thr_kill2, KTLS receive, sound mmap | Consumed by OPNsense 26.1.10/26.1.11 **[official]** |
| Stored XSS / injection fixes | OPNsense | Multiple GUI XSS + config-line injection fixes across 26.1.11 and 26.7 | Fixed in 26.1.11 and 26.7 **[official]** |
| "FortiBleed" credential dump | Fortinet (alleged) | Large credential set tied to Fortinet devices; no zero-day confirmed per EOTISEC | **[unverified]** |
| Underground FortiGate RCE "1-day" sale | Fortinet (claimed) | Private RCE sale claim, Sep 2026 | No CVE/reproduction — **[unverified]** |

---

## 6. Zero-trust / SASE 2026 developments (brief)

- **Fortinet** is driving the convergence narrative with the **"SASE Firewall"** framing (Q2 2026 earnings): firewall, SD-WAN and SASE integrated on a single FortiOS, powered by FortiASIC, deployable on-prem/cloud/sovereign. **FortiSASE Outpost** (GA with the FortiGate 1200G, Q3 2026) brings cloud-delivered SASE enforcement into customer-controlled environments for data-sovereignty and latency-sensitive traffic; **sovereign SASE deployment options** shipped in FortiOS 8.0 **[official/secondary — https://www.globenewswire.com/news-release/2026/07/29/3335555/0/en/fortinet-reports-strong-second-quarter-2026-financial-results.html; https://www.itp.net/cybersecurity/fortinet-launches-fortigate-1200g-to-advance-firewall-and-sase-convergence]**.
- **Fortinet ZTNA**: FortiOS 8.0 adds FortiGate-as-ZTNA-service-connector and IPv6 posture tags (official release notes); named a Leader in the **2026 Gartner Magic Quadrant for Hybrid Mesh Firewall** **[official/secondary]**.
- **pfSense**: no dedicated SASE/zero-trust product in 2026; ZTNA-equivalent access continues via OpenVPN/WireGuard/IPsec VPN on the appliance; management-plane direction is the Nexus controller + MIM entitlements **[official — forum]**.
- **OPNsense**: zero-trust story in 2026 is identity-driven: **improved OpenID Connect** in BE 26.4 (identity-claim visibility, Microsoft Entra ID integration), OPNcentral OIDC login flexibility, and OPNWAF diagnostics — no standalone SASE offering; commercial tier remains the Business Edition **[official — https://github.com/opnsense/docs/blob/HEAD/source/releases/BE_26.4.rst]**.

---

## 7. Open verification items

1. Fortinet: Intel Foundry as manufacturer of SP6 and "first external cybersecurity customer" — from a single secondary (TradingVision/UDIS) relay; not confirmed via Intel or Fortinet primary sources in this research.
2. Fortinet: exact throughput specs of the FortiGate 3500G and 400G (press release body not fully fetched; only architecture positioning captured).
3. Fortinet: full-year 2026 results not yet published as of Sep 22, 2026 (Q3 earnings expected late October).
4. "FortiBleed": no confirmed zero-day per the EOTISEC report itself; credential-set provenance unverified.
5. Underground FortiGate RCE sale (Sep 2026): unverified exploit-sale claim, no CVE.
6. pfSense CE 2.9.0: reported via community/GitHub matrices only; not verified against Netgate's official release notes.
7. pfSense Plus 26.10: listed as TBD in Netgate's version table; not released as of Sep 22, 2026.
8. Netgate MIM per-device yearly entitlement pricing details (exact price tiers) not located; community FAQ references exist but exact SKUs unverified.
9. OPNsense: 2026 download/user-count/adoption figures not located; Deciso does not publish them in the fetched sources.
10. NGFW market-size estimates vary widely ($5.76B–$7.12B for 2026) depending on the analyst firm; treat as ranges, not facts.
11. OPNsense 26.1 initial release date inferred (~Jan 28, 2026) from "upgrade path unlocked Jan 29"; exact GA date not stated in the fetched release notes.

## 8. Collection metadata

- **Collected:** September 22, 2026.
- **Sources:** Fortinet official docs/earnings (docs.fortinet.com, GlobeNewswire), Netgate forum + docs (forum.netgate.com, docs.netgate.com), OPNsense official docs (docs.opnsense.org) + Deciso PR, BleepingComputer-via-TechRadar, Dark Reading, InfoRiskToday, EOTISEC analytical report, market research aggregators, CSO Online, SecurityBrief, Linuxiac, cybersecuritynews.com.
- **Conventions:** all monetary figures in USD unless noted; prices are Sep 2026 snapshots; every version number carries a release date where verified.

---

## 9. Supplementary research pass — additional findings (second track-D agent, 2026-09-22)

### 9.1 FortiSOC — agentic-AI SOC platform details (announced June 16, 2026)

- **FortiSOC** launched June 16, 2026 (Sunnyvale, Calif.), cloud-delivered (SaaS) platform unifying SIEM, SOAR, threat intelligence, UEBA, case management, and ITDR **[official — https://www.fortinet.com/corporate/about-us/newsroom/press-releases/2026/fortinet-launches-fortisoc-unified-soc-platform-powered-by-agentic-ai]**.
- Agentic layer branded **FortiAI-Assist**: autonomous alert investigation, cross-asset/identity correlation, AI-generated playbooks, **Model Context Protocol (MCP)**-powered agent coordination across alerts, investigations, threat hunting, cases and response **[official]**.
- Built-in detection content/playbooks from Fortinet's own global SOC, with **monthly content updates** **[official]**.
- Previewed at **Fortinet Accelerate 2026** as the successor architecture to FortiAnalyzer/FortiSIEM/FortiSOAR/FortiTIP, which continue to be sold for customers preferring them **[secondary — https://investingnews.com/fortinet-advances-its-security-operations-platform-with-unified-soc-agentic-ai-and-expanded-endpoint-security/]**.
- FortiGuard SOC-as-a-Service enhancements: third-party log sources, FortiNDR and FortiCNAPP telemetry **[secondary]**.
- Positions Fortinet directly against Palo Alto Networks, CrowdStrike, Microsoft Sentinel in XDR/SIEM **[independent — https://cyber.netsecops.io/articles/fortinet-launches-unified-fortisoc-platform-with-agentic-ai/]**.

### 9.2 FortiOS 8.0 release-notes confirmations (official docs)

- **FortiAI-Assist** is documented as Feature ID 1058390 in the FortiOS 8.0.0 release notes: RAG-enhanced docs support, automated diagnostic analysis, CLI Code Lab with natural-language config generation; uses FortiAI with monthly FortiCare Premium token allotments (top-up possible) or customer OpenAI keys **[official — https://fortinetweb.s3.amazonaws.com/docs.fortinet.com/v2/attachments/d94b0a41-2dee-11f1-b31d-02356ffb40d9/fortios-v8.0.0-release-notes.pdf]**.
- New `http_authd` daemon for administrative authentication; GUI-driven FortiAP packet sniffer and PCAP export; granular federated-upgrade failure reporting **[official]**.

### 9.3 FortiGate 1200G / 3500G / 400G spec confirmations

- **FortiGate 1200G**: 397 Gbps firewall throughput, 10G/25G/100G connectivity, availability expected Q3 2026; integrates FortiGuard AI-Powered Security Services + FortiAI **[secondary — https://www.csoonline.com/article/4202566/fortinets-new-fortigate-platform-converges-firewall-sase-technologies-2.html; https://www.itp.net/cybersecurity/fortinet-launches-fortigate-1200g-to-advance-firewall-and-sase-convergence]**.
- **FortiGate 3500G**: ~595 Gbps firewall throughput, ~179M concurrent sessions (data-center scale, 400Gb connectivity) **[secondary — partner-published; vendor-derived]**.
- **FortiGate 400G**: ~164 Gbps firewall throughput, ~28M concurrent sessions (enterprise edge) **[secondary — partner-published; vendor-derived]**.

### 9.4 CVE-2026-35616 — FortiClient EMS authentication bypass ("FortiBleed" exploitation thread)

- Actively exploited FortiClient EMS auth-bypass flaw, patched **April 2026**; Fortinet advisory linked in PoC documentation **[secondary — https://github.com/intelseclab/poc-archive/blob/HEAD/pocs/network/2026-07-03_cve-2026-35616-forticlient-ems-auth-bypass/README.md]**.
- Attack technique: enumerate client-cert CA names via `openssl s_client`, forge a self-signed certificate, replay forged `X-SSL-CLIENT-VERIFY` header against `/api/v1/fabric_device_auth/*` and `/api/v1/fortigate/*` endpoints — no real mTLS handshake required **[secondary]**.
- **Remediation: upgrade FortiClient EMS to 7.4.7 or later**; interim: restrict management-plane access to trusted admin networks **[secondary]**.
- Linked to the "FortiBleed" credential-theft campaign and **INC Ransom / Lynx ransomware** operations (BleepingComputer, July 2026); EKZ Stealer artifacts noted downstream **[secondary — https://www.bleepingcomputer.com/news/security/fortibleed-credential-theft-campaign-linked-to-lynx-ransomware/]**.

### 9.5 CVE-2024-21762 — Thai ISP (3BB) intrusion (observed June 3, 2026)

- Hunt.io discovered an exposed operator toolkit (92.63.180[.]133:8888; 298 files / 30 subdirs) targeting a **FortiGate 60F SSL-VPN** (mail.3bb.co[.]th:10443) **[independent — https://cyberpress.org/hackers-exploit-fortigate-ssl-vpn-flaw-to-breach-thai-isp-and-deploy-meshcentral-backdoor/; https://gbhackers.com/hackers-exploit-fortigate-ssl-vpn-flaw/]**.
- Attackers fingerprinted FortiOS builds, staged controlled service-crash tests to distinguish vulnerable appliances, then exploited **CVE-2024-21762** (SSL-VPN out-of-bounds write → unauthenticated RCE) via `/remote/hostcheck_validate` ROP payloads; reverse shell to 92.63.180[.]133:9443; attempted firmware download to tailor ROP gadgets to the target's model/firmware **[independent]**.
- Post-exploitation: **MeshCentral** deployed as C2 (group "TH-3BB"), agents running with root privileges **[independent]**.

### 9.6 Fortinet Q1 2026 context and financial framing

- Q1 2026 (reported May 6, 2026): beat estimates; billings **+31% YoY**; announced FortiGate 3500G/400G alongside; collaboration noted with **Anthropic (Project Glasswing)** and OpenAI Group on AI security (scope undisclosed) **[secondary — https://siliconangle.com/2026/05/06/fortinet-shares-surge-first-quarter-earnings-beat-raised-outlook/]**.
- Q1 guidance at the time raised FY2026 outlook to revenue **$7.71–7.87B**, billings $8.80–9.10B, adjusted EPS $3.10–3.16 **[secondary]**.
- Q2 2026 segment detail (per earnings coverage): Secure Networking billings **+34%** (~66% of billings); Unified SASE **+35%** (~24%); AI-driven SecOps **+25%** (~10%); OT **+55%+**; **FortiSASE billings +100%+ YoY**; total billings **$2.37B (+33.4%)**; revenue **$2.05B (+25.6–26%)**; product revenue $773M (+52%); service revenue $1.27B (+14%); deferred revenue $7.68B **[independent — https://www.zacks.com/stock/news/2964411/fortinet-q2-earnings-revenues-beat-estimates-increase-yy; https://infotechlead.com/security/fortinet-q2-2026-revenue-jumps-26-to-2-05-bn-as-ai-data-center-sase-and-ot-security-demand-accelerates-97371]**.
- Combined "SASE Firewall" (NGFW + SD-WAN + Unified SASE + AI/quantum) grew **34%** and represented ~90% of billings; Fortinet's TAM claims: $148B by 2029 (SASE Firewall), $69B Secure Networking, $79B Unified SASE, $167B AI-driven SecOps **[vendor-reported]**.
- Stock: up ~12% in the month after Q2 results (Aug 28, 2026); traded near all-time highs ~$153–$174 with 52-week range $73.55–$174.05; ~96% one-year gains; forward P/E ~40–60 **[secondary — https://www.zacks.com/stock/news/2981878/why-is-fortinet-ftnt-up-12-since-last-earnings-report; https://tickeron.com/blogs/fortinet-ftnt-and-palo-alto-networks-panw-cybersecurity-sector-comparison-16826/; https://www.ad-hoc-news.de/boerse/corporate-news/fortinet-inc-stock-trades-near-record-high-as-analysts-back-innovation/70109912]**.
- NYC **Innovation Hub** opened 2026 (training labs, demos, executive briefing, Fortinet Cloud PoP) **[secondary]**.

### 9.7 pfSense additional confirmations

- **pfSense Plus 26.07 (2026-08-13)** and **CE 2.9.0 (2026-08-20)** dates confirmed via GitHub MCP-server compatibility PR **[independent — https://github.com/gensecaihq/pfsense-mcp-server/pull/89]**.
- CE 2.9.0 base OS **FreeBSD 16.0-CURRENT** (lab-verified by a third-party project; same pfREST v2.10 contract across CE 2.8.1/2.9.0 and Plus 26.07) **[independent]**.
- Forum friction (Sept 2026): users whose Plus licenses (e.g., on SG2100) no longer renew discussing downgrade to free CE 2.9.0; "lifetime" era of the 2022 limited-time Plus promotion confirmed over **[secondary — https://forum.netgate.com/topic/201119/netgate-releases-pfsense-plus-software-version-26.07]**.
- **Netgate 1100**: $269 (TAC Lite included); TAC Professional 1-yr $49 / 2-yr $98 add-ons **[secondary — https://shop.netgate.com/products/1100-pfsense]**.
- **Netgate TAC on AWS**: Pro **$399/yr** (24h email SLA), Enterprise **$799/yr** (4h email/phone SLA), claimed 98% satisfaction **[vendor-reported — https://aws.amazon.com/marketplace/pp/prodview-lstvktrnia26i]**.
- Practitioner comparison (Wundertech, ~Aug 2026): OPNsense on fixed Jan/July cadence with bi-weekly security updates vs pfSense's irregular, Netgate-hardware-prioritized cadence; pfSense still recommended for complete beginners due to docs/community; OPNsense preferred interface; Zenarmor gives OPNsense a layer-7 story pfSense lacks **[independent — https://www.wundertech.net/pfsense-vs-opnsense/]**.

### 9.8 OPNsense additional confirmations

- **26.7.4 released September 15–16, 2026** — latest supported release at research date; 26.1 EOL July 15, 2026 **[official — https://forum.opnsense.org/Administrative/Announcements; https://en.wikipedia.org/wiki/OPNsense]**.
- Point releases: 26.7.1 (Jul 23), 26.7.2 (Aug 13), 26.7.3 (Sep 3), 26.7.4 (Sep 15) **[official — forum announcements]**.
- **Business Edition 26.4.2 released September 18, 2026**; BE 26.4 (Jun 3), 26.4.1 (Jul 23) **[official — forum announcements]**.
- **Nordian partners with Deciso** to scale European cybersecurity innovation — announced June 30, 2026 **[official — forum announcement]**.
- Ongoing 26.7-series issues: Tailscale-behind-NAT control-plane connectivity loss after 26.7.1_1 (GitHub issue opnsense/plugins#5613, Aug 2026, open); ports/tooling upgrade hiccups (Aug 2026) **[secondary]**.

### 9.9 Market figures (additional)

- NGFW software-solutions market: **$10.22B in 2026** → $11.16B (2027) per one vendor-estimate; North America $2.46B (2026), US $1.71B; Europe $1.75B (2026) **[secondary — https://www.fortunebusinessinsights.com/next-generation-firewall-market-112358; https://datainsightsmarket.com referenced estimates]**.
- Gartner **2026 Magic Quadrant for Hybrid Mesh Firewall** (Sept 2026): Fortinet, Palo Alto Networks, Check Point named Leaders; Fortinet highest on Ability to Execute, second consecutive year **[secondary — https://www.ad-hoc-news.de/boerse/corporate-news/fortinet-inc-stock-trades-near-record-high-as-analysts-back-innovation/70109912]**.
- 2026 buyer guidance (GBHackers): FortiGate safest broad price-performance pick; Palo Alto Networks where security depth outranks budget; Check Point where tested prevention accuracy matters; Zscaler for cloud-first; treat any NGFW's patching SLA as part of the purchase decision given edge devices are primary initial-access targets **[independent — https://gbhackers.com/ngfw-solutions-compared-features-pricing/]**.

### 9.10 Open verification items (supplementary)

1. 3500G/400G throughput specs (~595/164 Gbps) are partner-published, not from a Fortinet datasheet.
2. Plus 26.07 / CE 2.9.0 release dates derived from GitHub PR + forum, not the Netgate blog body.
3. FortiSOC real-world adoption/price figures not located.
4. CVE-2026-35616 FortiBleed-to-ransomware linkage rests on PoC-archive and press, not a Fortinet advisory on the campaign.
5. Stock/valuation figures are snapshots dated Jul–Sep 2026 and are time-sensitive.

- **Collected:** September 22, 2026.
- **Sources:** Fortinet official docs/earnings (docs.fortinet.com, GlobeNewswire), Netgate forum + docs (forum.netgate.com, docs.netgate.com), OPNsense official docs (docs.opnsense.org) + Deciso PR, BleepingComputer-via-TechRadar, Dark Reading, InfoRiskToday, EOTISEC analytical report, market research aggregators, CSO Online, SecurityBrief, Linuxiac, cybersecuritynews.com.
- **Conventions:** all monetary figures in USD unless noted; prices are Sep 2026 snapshots; every version number carries a release date where verified.
