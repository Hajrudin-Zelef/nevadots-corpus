---
id: collect-261121-veille-ia/reseau-networking/github-shaileshsahani-ccnp-enterprise-labs-gns3-hands-on-ccnp-enterprise-labs-built-with-gns3-coveri
title: "GitHub - ShaileshSahani/CCNP-Enterprise-Labs-GNS3: Hands-on CCNP Enterprise labs built with GNS3 covering OSPF, EIGRP, BGP, MPLS, VPNs, High Availability, Security, and Enterprise Network Design."
domain: reseau-networking
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-08-09"]
keywords: ["enterprise", "ospf", "eigrp", "mpls", "ccnp", "design", "route", "advanced"]
source: https://github.com/shaileshsahani/ccnp-enterprise-labs-gns3
source_anchor: ""
source_lines: [1, 213]
section: "Copilot_Microsoft"
---
A structured collection of **50 advanced CCNP Enterprise networking labs**, built in GNS3 on real Cisco virtual platforms. Progresses from intermediate redistribution scenarios to full enterprise capstone integration — routing, BGP, VRF, MPLS, WAN/VPN, security, and automation.

- Learn enterprise networking through hands-on, failure-driven labs
- Build real-world troubleshooting reflexes, not just working configs
- Master Cisco routing, switching, and redistribution at CCNP depth
- Prepare for CCNP Enterprise certification (ENCOR + ENARSI)
- Build a portfolio-grade record of applied network engineering

```
Foundation (CCNA)  →  Intermediate (CCNP Core)  →  Advanced (CCNP Expert)
        →  Architect (Enterprise Design)  →  Capstone (Full Integration)
```
| Resource | Minimum | Recommended | 
|---|---|---|
| CPU | 8 cores | 16+ cores | 
| RAM | 16 GB | 32+ GB | 
| Storage | 100 GB SSD | 250+ GB SSD | 
| Hypervisor | VMware Workstation / KVM | VMware Workstation Pro | 
| GNS3 | 2.2.x | 2.2.55+ | 

| # | Lab | Routers | Key Focus | 
|---|---|---|---|
| 01 | OSPF Fast Convergence & BFD | 4 | BFD, timers, convergence tuning | 
| 02 | OSPF Deep Troubleshooting | 4 | Adjacency states, LSDB analysis, LSA types | 
| 03 | OSPF LSA Manipulation | 4 | LSA control, filtering, type manipulation | 
| 04 | OSPF Area Design Failure | 5 | ABR/backbone failure scenarios | 
| 05 | EIGRP Advanced Troubleshooting | 4 | Metric analysis, feasibility condition | 
| 06 | EIGRP Named Mode & Authentication | 3 | Named EIGRP, SHA/MD5 auth | 
| 07 | Full OSPF–EIGRP–BGP Redistribution | 5 | Multi-protocol redistribution | 
| 08 | Redistribution Loop Prevention | 5 | Route tags, filtering strategies | 
| 09 | Administrative Distance Engineering | 4 | Route-source manipulation | 
| 10 | Recursive Routing Failure Analysis | 4 | Next-hop resolution troubleshooting | 

| # | Lab | Routers | Key Focus | 
|---|---|---|---|
| 11 | iBGP Full-Mesh Enterprise | 5 | iBGP, split-horizon rules | 
| 12 | BGP Path Selection Master Lab | 5 | Weight, Local Pref, AS Path, MED | 
| 13 | BGP Route Filtering Master Lab | 5 | Prefix-list, route-map, filter-list | 
| 14 | BGP Route Reflector | 5 | Route reflection hierarchy | 
| 15 | BGP Route Reflector Redundancy | 6 | Dual RR design | 
| 16 | BGP Confederation | 6 | Large-scale iBGP confederation | 
| 17 | BGP Local Preference Engineering | 4 | Outbound traffic engineering | 
| 18 | BGP AS-Path Prepending | 4 | Inbound traffic engineering | 
| 19 | BGP MED Multi-Exit Design | 5 | Multi-exit ISP scenarios | 
| 20 | Dual-ISP Enterprise BGP | 5 + 2 SW | Dual ISP, failover, NAT | 

| # | Lab | Routers | Key Focus | 
|---|---|---|---|
| 21 | VRF-Lite Multi-Tenant Enterprise | 4 + 2 SW | Tenant isolation | 
| 22 | VRF + OSPF | 4 | Per-VRF OSPF process | 
| 23 | VRF + EIGRP | 4 | Multi-VRF EIGRP | 
| 24 | VRF + BGP | 5 | Multi-VRF BGP | 
| 25 | Inter-VRF Route Leaking | 4 | Controlled route sharing | 
| 26 | Shared Services VRF Architecture | 4 + 2 SW | Shared services w/ Linux | 

| # | Lab | Routers | Key Focus | 
|---|---|---|---|
| 27 | IS-IS Fundamentals | 5 | L1/L2, NET addressing | 
| 28 | IS-IS Advanced Troubleshooting | 5 | LSDB, adjacency issues | 
| 29 | MPLS Fundamentals | 5 | MPLS labels, LDP | 
| 30 | MPLS L3VPN | 6 | PE/P/CE architecture | 
| 31 | MPLS Multi-Customer L3VPN | 6 | Multiple customer VRFs | 
| 32 | MP-BGP VPNv4 | 6 | VPNv4 address family | 
| 33 | MPLS L3VPN Failure Troubleshooting | 6 | PE/P/CE failure scenarios | 

| # | Lab | Routers | Key Focus | 
|---|---|---|---|
| 34 | Advanced GRE + Dynamic Routing | 4 | GRE + OSPF/EIGRP | 
| 35 | GRE + IPsec + OSPF | 4 | Encrypted dynamic routing | 
| 36 | DMVPN Phase 1 | 4 | Hub/spoke, NHRP | 
| 37 | DMVPN Phase 2 | 4 | Spoke-to-spoke | 
| 38 | DMVPN Phase 3 | 4 | NHRP redirect | 
| 39 | DMVPN + Dynamic Routing | 5 | DMVPN + EIGRP/OSPF | 

| # | Lab | Routers | Switches | Firewall | Linux | Key Focus | 
|---|---|---|---|---|---|---|
| 40 | Advanced IPsec Troubleshooting | 4 | 0 | 0 | 0 | IKE/IPsec failures | 
| 41 | AAA with TACACS+ | 3 | 1 | 0 | 1 | Centralized AAA | 
| 42 | AAA with RADIUS | 3 | 1 | 0 | 1 | RADIUS authentication | 
| 43 | AAA Failure & Local Fallback | 3 | 1 | 0 | 1 | AAA resilience | 
| 44 | Firewall + DMZ Enterprise Design | 3 | 2 | 1 | 1 | DMZ, NAT, ACL | 
| 45 | Firewall + NAT + VPN Integration | 3 | 2 | 1 | 1 | Security integration | 

| # | Lab | Routers | Switches | Linux | Key Focus | 
|---|---|---|---|---|---|
| 46 | Linux Network Services Integration | 3 | 1 | 1–2 | DNS, DHCP, Syslog, NTP | 
| 47 | Python Netmiko Multi-Device Automation | 4 | 1 | 1 | Python automation | 
| 48 | Ansible Network Automation | 4 | 1 | 1 | Infrastructure as Code | 
| 49 | NETCONF/RESTCONF Automation | 3 | 1 | 1 | API-driven automation | 

| # | Lab | Routers | Switches | Firewall | Linux | Key Focus | 
|---|---|---|---|---|---|---|
| 50 | Ultimate Enterprise Network Capstone | 8 | 3 | 1 | 2 | Full enterprise integration | 

**Routing** — Static, OSPF (multi-area, stub, NSSA, virtual links), EIGRP (classic/named, stub, summarization), BGP (eBGP, iBGP, route reflectors, confederations), IS-IS, redistribution, PBR, VRF-Lite, MPLS L3VPN

**Switching** — VLANs, STP (802.1D, Rapid-PVST+), EtherChannel (LACP/PAgP), inter-VLAN routing (ROAS, SVI), port security, private VLANs

**High Availability** — HSRP, VRRP, GLBP, BFD, fast convergence tuning

**WAN & VPN** — GRE, GRE over IPsec, DMVPN (Phase 1–3), site-to-site VPN, dual-ISP connectivity, NAT (static/dynamic/PAT)

**Network Services** — DHCP server & relay, DNS, syslog, SNMP (v2c/v3), NTP, NetFlow/sFlow, AAA (TACACS+/RADIUS)

**Security** — Standard/extended ACLs, port security, DHCP snooping, DAI, IP source guard, 802.1X, CoPP, management plane protection, firewall policies, DMZ design, NAT/PAT

**Automation** — Python (Netmiko, Paramiko), Ansible, NETCONF, RESTCONF, YANG models, Git

```
CCNP-Enterprise-Advanced-Labs/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
│
├── Lab-01-OSPF-BFD-Fast-Convergence/
│   ├── README.md              # lab objective, topology, IP plan
│   ├── topology.png
│   ├── configs/
│   │   ├── R1.cfg
│   │   ├── R2.cfg
│   │   ├── R3.cfg
│   │   └── R4.cfg
│   ├── verification.md
│   ├── troubleshooting.md
│   └── gns3-project/
│
├── Lab-02 .. Lab-49/
│   └── (same structure)
│
├── Lab-50-Ultimate-Enterprise-Capstone/
│   └── (same structure)
│
└── Images/
    └── Topology-Diagrams/
```
Each lab folder contains: objective, topology diagram, IP addressing table, complete configs, verification commands, troubleshooting guide, real-world use case, and key takeaways.

| Component | Version / Details | 
|---|---|
| GNS3 | 2.2.55+ | 
| OS | Ubuntu 20.04/22.04 LTS | 
| Hypervisor | VMware Workstation Pro 16/17 or KVM | 
| Cisco Images | IOS 15.x / IOS-XE 16.x / 17.x | 
| Linux VMs | Ubuntu Server 20.04/22.04, Alpine | 
| Firewall | Cisco ASA, pfSense, or iptables | 

Cisco IOS images are **not included**. Supply your own appropriately licensed images.


Required images (examples):

```
c7200-adventerprisek9-mz.152-4.S.bin
i86bi-linux-l3-adventerprisek9-15.5.2T.bin   (IOSv)
vios_l2-adventerprisek9-m.ssa.high_iron_20190423.bin   (IOSvL2)
asav981-7.qcow2   (ASAv, optional)
```
```
# 1. Install GNS3
sudo add-apt-repository ppa:gns3/ppa
sudo apt update
sudo apt install gns3-gui gns3-server
# 2. Install a hypervisor
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
# 3. Clone the repo
git clone https://github.com/ShaileshSahani/CCNP-Enterprise-Advanced-Labs.git
cd CCNP-Enterprise-Advanced-Labs
# 4. Import Cisco images in GNS3
# Edit → Preferences → IOS routers → New → follow the import wizard
```
Then: pick a lab → read its README → build the topology in GNS3 → apply configs → verify → intentionally break something → fix it.

| Level | Labs | Routers | Focus | 
|---|---|---|---|
| Intermediate | 1–10 | 3–5 | Core routing, redistribution | 
| Advanced | 11–26 | 4–6 | BGP, VRF, multi-tenancy | 
| Expert | 27–45 | 4–6 | MPLS, WAN, security | 
| Architect | 46–50 | 3–8 | Automation, full integration | 

1. **Physical** — interfaces up/up?
2. **Layer 2** — VLANs, trunking, STP state
3. **Layer 3** — IP addressing, routing table
4. **Layer 4** — ACLs, firewall rules
5. **Application** — protocol-specific debugs

- CCNA-level networking knowledge
- TCP/IP and subnetting fluency
- Comfort with Cisco IOS CLI
- Basic GNS3 experience
- Working knowledge of routing protocol fundamentals

**References:** CCNP Enterprise ENCOR 350-401 & ENARSI 300-410 Official Cert Guides, TCP/IP Illustrated Vol. 1, Cisco Learning Network, GNS3 Documentation

- Lab structure defined (50 labs)
- README published
- Individual lab documentation (in progress)
- GNS3 project files per lab
- Configuration templates
- VXLAN EVPN labs
- SD-WAN concepts
- Segment Routing (SR-MPLS)
- IPv6 enterprise design
- Terraform for network automation
- CML/VIRL compatibility

**Shailesh Sahani**
Network Technician → Network Engineer | CCNP Enterprise (in progress)

If this helped your CCNP journey: star the repo, fork it, or open an issue with feedback.

MIT License — Copyright (c) 2026 Shailesh Sahani. See LICENSE for full text.

*Last updated: August 9, 2026*
