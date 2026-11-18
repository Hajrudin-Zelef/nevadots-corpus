Cisco Meraki Hybrid Campus LAN Design Guide
CIsco Meraki Enterprise Networking
Architecture, Deployment Options, Cloud Monitoring & Best Practices for a Scalable, Secure Enterprise Campus Network
1. What Is a Hybrid Campus LAN?
A Local Area Network (LAN) is the networking infrastructure that provides access to communication services and resources for end users and devices spread across a single floor, building, or campus. When multiple LANs spread across a local geographic area are interconnected, the result is a Campus Network — which can scale from a single switch in a small remote office all the way to thousands of switching ports in a large multi-building enterprise complex.
A Hybrid Campus LAN refers to a network design that deliberately combines two or more hardware and software platforms — specifically Cisco's Catalyst (DNA/Catalyst Center) portfolio and the Meraki cloud-managed portfolio — into a single, unified campus fabric. This "hybrid" mixing of platforms is extremely common in enterprise environments, whether due to phased upgrades, budget constraints, or the desire to benefit from both portfolios simultaneously.
🎯 What This Design Enables
🔗 Tiered LAN Connectivity
Access, distribution, and core layers designed for performance and scale.
📡 Wired + Wireless Ready
Infrastructure prepared for multimedia, VoIP, and high-density wireless.
📦 IP Multicast
Efficient data distribution for video, streaming, and enterprise applications.
🌐 WAN & Internet Edge
Core interconnection to WAN and internet via SD-WAN capable MX appliances.
2. Why Hybrid? Catalyst + Meraki Together
Cisco offers two distinct but complementary campus LAN product lines. Understanding why organizations choose a hybrid approach requires understanding what each brings to the table:
⚙️ Cisco Catalyst Portfolio
Powered by Cisco Catalyst Center (formerly DNA Center) — an on-premises automation, assurance, and security management platform.
- Network automation & programmability
- AI/ML-driven network assurance
- SD-Access segmentation
- Advanced security with Cisco ISE
- MACSec encryption at the port level
☁️ Cisco Meraki Portfolio
Powered by the Meraki Dashboard — a cloud-first management platform that simplifies deployment, visibility, and day-to-day operations.
- Zero-touch provisioning
- Unified dashboard across switching, wireless, SD-WAN
- Built-in security (UTM) and traffic analytics
- Easy remote management and troubleshooting
- Fast deployment with minimal expertise required
💡 Key Insight: The Hybrid Campus LAN approach is ideal for organizations that want to maximize value from both portfolios — leveraging Meraki's operational simplicity for day-to-day management while benefiting from Catalyst's advanced security, routing, and scalability at the core and distribution layers. Designing for interoperability between the two requires careful planning, which is exactly what this CVD provides.
3. Cloud Management & Monitoring for Cisco Catalyst
One of the cornerstone features of this CVD is the ability to bring Cisco Catalyst switching infrastructure into the Meraki Dashboard for unified visibility. This capability — known as Cloud Monitoring for Catalyst — allows network teams to see both Meraki-managed and Catalyst-managed devices in a single pane of glass.
Supported Catalyst Platforms for Cloud Monitoring
⚠️ Critical Limitation — Monitor Only, Not Manage
Cloud Monitoring for Catalyst provides visibility only. You can see device status, topology, and selected configuration parameters from the Meraki Dashboard — but you cannot push configuration changes to Catalyst devices via the dashboard. Full management of C9500 core switches remains via Cisco Catalyst Center or CLI.
Pre-Requisites for Cloud Monitoring Onboarding
The switch must be a supported model (C9200, C9300, or C9500)
Must have minimum firmware 17.3.1 or higher
Must have an SVI or routed interface with internet access on TCP port 443
Must have a valid DNS server configured
Must have a valid Cisco DNA / Catalyst Center subscription
📌 Note: The onboarding process for C9500 core switches is out of scope for the CVD itself. Refer to the Cisco Cloud Monitoring for Catalyst documentation for a step-by-step onboarding guide.
4. Hybrid Campus LAN Architecture & Components
The reference architecture validated in this CVD combines best-of-breed hardware from both the Meraki and Catalyst portfolios. Each component is purpose-selected for its role in the campus fabric.
MR55 / MR56 / MR57 & C9166-MR Access Points
Management: Meraki Dashboard | Standard: WiFi 6 (802.11ax)
MS390-24P & C9300-24P Access Switches
Management: Meraki Dashboard | Switching Capacity: 208 Gbps
C9500-24Y4C Collapsed Core Switch
Management: Monitor Only in Meraki Dashboard | Switching Capacity: 6.4 TB
MX250 SD-WAN / UTM Appliance (Warm-Spare HA)
Management: Meraki Dashboard | Throughput: 4 Gbps FW / 2 Gbps SD-WAN
📌 Note: Catalyst –M and –MR model SKUs are pre-shipped in Cloud Managed (Meraki) mode. Non-M models can be transitioned to Meraki management mode via CLI. C9300-M uses Cloud Managed mode; C9500 uses Monitor Only mode.
5. Logical Architecture: Three Design Options
This CVD defines three validated logical architecture options, each with different characteristics in terms of VLAN flexibility, convergence behavior, and STP configuration requirements. Choosing the right option depends on your operational priorities and tolerance for complexity.
6. Option 1 — Layer 2 Access with Native VLAN 1 (STP-Based)
In this design, the Spanning Tree Protocol (STP) domain is extended all the way from the access layer to the core layer. VLANs can span across multiple access switch stacks and closets, making this the most flexible option in terms of VLAN design. The recommended STP protocol for this hybrid environment is Multiple Spanning Tree Protocol (MST / 802.1s), as it is supported on both Meraki MS390 and Catalyst platforms.
✅ Pros
- Flexible VLAN design across the campus
- Facilitates seamless wireless roaming
- Easier and consistent configuration across all switches
- Well-suited for large, flat campus deployments
❌ Cons
- Non-deterministic route failover
- Slow STP convergence if not properly tuned
- Different STP protocol support on Catalyst vs Meraki
- Risk of VLAN hopping without proper hardening
⚠️ STP Tuning is Critical: Since Catalyst platforms can run different STP variants than Meraki MS390 switches, it is essential to standardize on MST (802.1s) across your entire campus STP domain. STP must be carefully tuned — including BPDUs, port types, root bridge priority, and convergence timers — to avoid loops and slow failover in a mixed-vendor environment.
7. Option 2 — Layer 2 Access without Native VLAN 1
This option is architecturally similar to Option 1, but with one important security distinction: Native VLAN 1 is replaced with a dedicated, non-trivial management VLAN. This separation reduces the risk of VLAN hopping attacks and aligns with security hardening best practices for enterprise campus LAN design.
✅ Pros
- All VLAN flexibility of Option 1
- Minimizes risk of VLAN hopping
- Management VLAN isolated from Native VLAN
- Preferred option for security-conscious organizations
❌ Cons
- Non-deterministic route failover (same as Option 1)
- Requires careful Native VLAN consistency across all switches
- Different STP protocol support remains a challenge
📌 Important Note: When using Cisco PVST (Per-VLAN STP) in any part of the network, VLAN 1 becomes essential for backward-compatible BPDU communication between switches. If VLAN 1 must be completely eliminated, ensure MST is consistently configured across all Meraki and Catalyst platforms in your STP domain to prevent unexpected BPDU behavior.
8. Option 3 — Layer 3 Access (Recommended for Scale)
In Layer 3 Access design, the IP routing boundary is pushed all the way down to the access layer. Instead of a flat Layer 2 STP domain spanning the entire campus, each access switch or stack terminates VLANs locally and routes traffic upward via Layer 3 links. This eliminates the non-deterministic nature of STP and replaces it with fast, predictable routing protocol convergence.
🏆 Why Layer 3 Access is Best for Large Deployments
⚡ Fast Convergence
Routing protocols converge faster and more predictably than STP in large networks.
🔒 Better Segmentation
Routed boundaries naturally contain broadcast domains and improve security posture.
📈 Better Scalability
No STP topology changes ripple across the entire campus — faults remain local.
🎯 Deterministic
Equal-cost multipath (ECMP) routing provides predictable load sharing and failover.
In this design, all SVIs (Switch Virtual Interfaces) are created at the collapsed core layer. The access switches act as pure Layer 3 forwarders, and wireless roaming across closets requires a Layer 3 Roaming Concentrator to maintain session continuity when clients move between access switch domains.
💡 Wireless Roaming Consideration: Layer 3 Access does not natively support seamless wireless roaming across campus zones without additional configuration. A Layer 3 Roaming Concentrator (tunnel-based roaming) must be deployed to anchor wireless clients as they move between different IP subnets served by different access switch stacks.
9. Security: 802.1x, Cisco ISE & Adaptive Policy
Security is deeply embedded throughout this CVD. The architecture supports both wired and wireless 802.1x authentication via RADIUS, integrated with Cisco ISE (Identity Services Engine) for policy enforcement, posture checking, and dynamic VLAN assignment.
802.1x & MAB Authentication
Access policies are configured through the Meraki Dashboard (Switching → Configure → Access Policies). The CVD validates two modes:
🔐 802.1x — EAP Authentication
Used for corporate devices supporting EAP. Users authenticate against Cisco ISE using credentials. ISE returns dynamic VLAN assignment and Security Group Tags (SGT) via RADIUS. CoA (Change of Authorization) is enabled to push policy updates post-authentication.
🔑 MAB — MAC Authentication Bypass
Fallback method for devices that don't support 802.1x (printers, IoT devices). The device MAC address is passed to ISE as credentials. ISE policy determines appropriate VLAN and access level based on the MAC address database.
Wireless SSID Design & ISE Integration
The CVD validates two SSIDs, each mapped to a distinct VLAN and ISE policy:
Adaptive Policy (SGT — Security Group Tagging)
The CVD also validates Adaptive Policy (Cisco's Meraki implementation of TrustSec SGTs) for micro-segmentation. Groups are defined in Meraki Dashboard (Organization → Configure → Adaptive Policy), and SGT values are assigned per user group. This allows policy-based access control independent of VLANs or IP addresses — making segmentation portable and scalable.
10. Quality of Service (QoS) Design
A properly designed QoS policy is essential for supporting real-time applications such as voice, video conferencing (Webex), and critical enterprise applications on a shared campus LAN. The CVD validates QoS end-to-end across both wired and wireless infrastructure.
QoS is configured via Meraki Dashboard (Wireless → Configure → Firewall & Traffic Shaping). Key DSCP markings validated in this CVD include:
✅ Validated Scenario: Wireless roaming was tested between two campus zones and APs homed to different switch stacks while maintaining an active Webex meeting with Audio, Video, and Content Share — confirming that QoS policies hold through roaming transitions and that voice/video quality is maintained.
11. High Availability & Redundancy Design
High availability is a fundamental design requirement for any enterprise campus LAN. The CVD incorporates HA principles at every layer of the architecture:
🔁 MX250 Warm-Spare (WAN Edge)
The MX250 WAN edge appliance is deployed in warm-spare (active/passive) configuration, requiring only a single license for both appliances. In the event of the primary MX failure, the secondary takes over seamlessly with no manual intervention.
🔗 Link Aggregation (EtherChannel / LACP)
LAG (802.3ad) is deployed between access switches and the distribution/core layer, providing higher bandwidth, load sharing, and link-level redundancy without relying on STP to handle failures.
🏗️ Redundant Triangles (Access-to-Core)
Best practice design uses redundant triangle topologies rather than redundant squares. Each access stack connects to two distribution/core switches via dual equal-cost paths, enabling fast failover without spanning tree loops.
⚡ StackPower for Access Switches
Cisco StackPower shares power budgets across stacked access switches, providing power redundancy so that a single power supply failure does not bring down the entire stack. Redundant power supplies are recommended across all campus switching layers.
12. Planning & Best Practices Checklist
Use this pre-deployment checklist to ensure your Hybrid Campus LAN implementation follows the validated design guidelines from this CVD:
☁️ Cloud & Platform
Determine if access switches will run in Cloud Managed (Meraki Dashboard full control) or Cloud Monitored (visibility only) mode
Ensure Catalyst switches meet the minimum firmware version 17.3.1 before onboarding to cloud monitoring
Verify TCP port 443 outbound internet access is available from all SVIs on Catalyst switches
Confirm valid DNS configuration on all Catalyst switches to be cloud-monitored
🏗️ Design & Topology
Select the appropriate logical design option (L2 VLAN1, L2 No VLAN1, or L3 Access) based on your scale, security, and roaming requirements
Standardize on MST (802.1s) as the STP protocol across all Meraki and Catalyst platforms in the same STP domain
Deploy redundant triangles from access stacks to core, not redundant squares
If using Layer 3 Access, plan for a Layer 3 Roaming Concentrator to support seamless wireless mobility
🔐 Security
Configure 802.1x and MAB access policies in Meraki Dashboard for both wired and wireless clients
Integrate Cisco ISE for RADIUS authentication, dynamic VLAN assignment, SGT, and posture checking
Enable RADIUS CoA to allow ISE to dynamically change client authorization post-connection
Enable STP BPDU Guard on access ports to protect the STP topology from unauthorized devices
📡 QoS & Wireless
Configure DSCP markings end-to-end (EF for VoIP, AF41 for video, AF11 for critical data)
Test wireless roaming across multiple zones with an active real-time application (video call) before go-live
Deploy WiFi 6 (802.11ax) access points for high-density environments and future-proof wireless coverage
13. Conclusion
The Cisco Meraki Hybrid Campus LAN CVD represents one of the most comprehensive and practically validated design guides available for enterprise network architects. By combining the operational simplicity of the Meraki Dashboard with the raw performance and advanced security of the Catalyst 9000 series, organizations can build campus networks that are simultaneously easy to manage, secure, scalable, and ready for the future.
Whether you choose Layer 2 with MST for maximum VLAN flexibility, the security-hardened no-VLAN-1 design, or the Layer 3 Access model for deterministic convergence and scale — this CVD provides a pre-validated path with documented testing results, verified configurations, and clear design trade-offs.
The validated integration of Cisco ISE, Adaptive Policy/SGT, QoS, WiFi 6, and Cloud Monitoring makes this design a complete enterprise-grade campus blueprint — not just a reference diagram. For organizations modernizing their campus networks, this CVD is the starting point, not the destination.
Want to go deeper into Cisco Campus Design?
Explore the full official Cisco Meraki documentation and Cisco CVD for complete configuration examples, topology diagrams, and verification steps.
Meraki Docs ↗ Cisco CVD ↗
Tags