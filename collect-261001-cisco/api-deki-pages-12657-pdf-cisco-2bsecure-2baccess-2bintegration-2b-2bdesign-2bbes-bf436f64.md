Cisco Secure Access Integration - Design Best Practices
Cisco Meraki with Secure Access integration design best practices article outlines options for deploying Meraki MX SD-
WAN hub and spoke networks with Secure Access. Following these recommended designs will help ensure your
network achieves optimal performance and security. Listed designs had been validated with scale, optimal operation
and resiliency in mind.
Overview
Today’s remote and hybrid workforce, widespread cloud adoption, and increased internet-bound traffic require organizations to deliver secure, optimized access
to applications anywhere, on any device. Traditional network models struggle to keep pace, prompting organizations to embrace Secure Access Service Edge
(SASE) architectures. By integrating Cisco Meraki SD-WAN with Cisco Secure Access, businesses benefit from unified, cloud-native networking and security,
ensuring consistent protection, simplified operations, and a scalable user experience across all locations.
Cisco Merakiand Secure Accessstand out with their centralized, cloud-based dashboards, enabling IT teams to deploy, monitor, and manage networks from
anywhere—eliminatingthe need to build moreon-premises controllers. The platform integration prioritizes simplicity, scalability, and automation, while offering
built-in analytics and security for both enterprise and distributed environments. Managed through the Cisco Meraki dashboard, Meraki MX SD-WAN leverages
AutoVPN technology to seamlessly orchestrate and provision Secure Access cloud hubs and tunnels between sites in a spoke-and-hub network.
This design best practices document outlines options for deploying Meraki MX SD-WAN with Secure Access to deliver a comprehensive SASE solution.
Following these recommended designs will help ensure your network achieves optimal performance and security.
Dual Fabric Terminology
For clearer guidance, the following special terms describe devices and traffic paths within two SD-WAN fabrics:Traditional Cisco Meraki MX SD-WAN and
Cisco SSE Secure Access.
Cloud Fabric: Transport via cloud hubs using Cisco Secure Access; cloud path or cloud transport
Cloud Spoke: Cisco Meraki spoke connected (enrolled) exclusively to the cloud
Cloud Hub: Cisco Secure Access-enhanced head-ends that provide cloud transport and are designated with the CPSC-HUB platform type in Cisco
Meraki MX Dashboard
MX (Local) Fabric: Transport via MX hubs, using traditional Cisco Meraki MX SD-WAN tunnels and routing; also referred to as the MX path or MX
transport
Local Spoke: MX in spoke mode operating solely in the MX fabric, listing local or enrolled hubs
Local Hub: MX in hub mode that appears in a spoke's hub priority list
Dual-fabric devices:
Hybrid Spoke: MX spoke that uses both cloud fabric and MX fabric for traffic transport
Hybrid (Enrolled) Hub: MX hub that forms a mesh with other MX hubs while enrolled in the cloud fabric and route peering (hub-to-hub) with the
region’s pair of cloud hubs
Further explanation on thesetermscan be found in the Secure ConnectHub Integrationdocument.
1

Cisco SASE Supported Topologies for Meraki
The following diagrams show the difference between spoke-spoke communication when enabling secure private access for users.
Example Meraki topology:
Example Meraki topology via Secure Access:
2

Why do these changes occur?
When deploying a SASE architecture, it is recommended to inspect East-West traffic between sites to maximize the security efficacy of the security services.
Secure Access integration with Meraki orgdisables the direct Spoke-Spoke communication via MX Hub (2) to ensure all traffic is inspected, and policy is applied
as intended (1).
Platform Optimization
To improve platform stability and resiliency, the following optimization is adopted at onboarding for all organizations that enable the Meraki Secure Access
integration:
• Dashboard installed routes are removed from all Spokes and Hubs.
• BGP protocol is used as a sole method of providing routes to sites
• Meraki Hubs are prevented from sharing the Spoke routes they learned toward other Spokes.
• Cloud Spokes traffic to any other sites (any Spoke or Hub) prefers the cloud path
The table below describes the Cloud vs. Local tunnel path used by end points as they communicate.
* MX Hub (Hybrid or Local) must be configured on a Spoke to enable direct local-path access to its prefixes.
Scale of Sites and Routes in the Meraki Secure Access Solution
Building sizable SD-WAN site topologies for Meraki with Secure Access requires careful consideration for routes scale on all devices. MX Hubs must stay
under10,000-route limit while Secure Access Cloud Hubs need to stay below 70,000-route limit. This allows unoptimized organizationsto support up to1000
enrolled Sites, with each advertising a maximum of10 routing prefixes to the Cloud Fabric. These MX-advertised prefixes encompass local subnets, eBGP-
learned routes, and defined static routes. In this case, Cloud and HybridSpokes will receive a default route and specific routesknown by the Cloud Hubs.
For quicker convergence with Cloud Hubs, platform can be optimized so Spoke sites can be configured to receiveonly a default route, bypassing specific iBGP
route prefixes. This simplified routing also allows sites to advertise an increased limit of up to28 prefixes with the2500 enrolled Sites. To enable this reduced
routing configuration for your regions, please contact Meraki support.
Organizations deploying below Design B at scale (Hybrid Spokes with Hybrid and Cloud Hubs) can accommodate max of2 MX Hybrid Hubs. For organizations
3

using asmall number of Sites, more MX Hybrid Hubs can be enrolled in Secure Access. The SitesUInow allows 100 hubs enrolled, validated by the
Design D (Hybrid Hubs with Cloud Hubs).
Use of templates significantly improves enrollment time ofSites. WhenSites do not use templates, theyshould be enrolled ingroups ofno more
than200Sites. Each batch should completelyenrollbefore proceeding to enroll more sites. Contact Meraki Support for assistance.
Learn more about the 2025 Platform Optimization here, implemented first with Secure Connect.
These settings, derived from insights gained from the Secure Connect product and its underlying technology, ensure faster failover during Data Center outages.
They also simplify spoke site routing, thereby reducing overall route complexity and improvingBGP convergence.
Do I need a Maintenance Window?
• Do you have the Secure AccessSecure Internet Access package only (No SPA package purchased)?
• Customers with the following packagesmay need a maintenance window:
• Secure Internet Access (SIA) Essentials
• Secure Internet Access (SIA) Advantage
• Customers with the following packageswill not need a maintenance window:
• Secure Internet Access (SIA) + Secure Private Access (SPA)
• DNS Defense is not yet supported via this integration.
• Does your organization requirespoke-spoke communication using an MX hub?
• Every Meraki organization has this capability by default, but not all customers use it.
• Some larger organizations use this to manage the scalability of their deployments when changing routing configuration.
To addressbroaderdeployment needs, increasedscale forSpokes and Hubs is beingvalidated and updated in this document.If the design you
intend to use is not documented here, please contact your account team.
If you have answeredYes to both questions above, you should schedule a maintenance window BEFORE enabling the Secure Access integration.
• In case your Meraki organizationrequires MX Hubs mesh feature to beDISABLED?
◦ Every Meraki organization has the Hub mesh enabledby default, but customers can ask Meraki
support to disable this feature.
◦ Secure Access integration onboarding re-enables MX Hub mesh toenablesupportforMX Hub
enrollment into SSE as part of Design B.
◦ If your organization has MX Hub mesh disabled, and you are onboarding to Secure Access integration,
4

Enabling Secure Access Meraki Integration in a Maintenance Window
Once you are in your scheduled maintenance window, complete the following steps:
1. Turn on the integration by completingthese steps
2. Onboard the hubs & spokes to SSE by completingthese steps
3. If you have SIA onlyand desire Spoke-Spoke communication,call support to have them disable the feature after you have onboarded your hubs &
spokes to Secure Access.
• You can reference this document or tell them that you “desire Spoke-Spoke communication” reinstated on your organization.
4.Optional: Organizations managing a large number of sites (over 500) enrolled in a single Secure Access region can enable theonly-default-route for Spokes
feature described in the Platform Optimization section of this document. Customerscan request to enable this feature when they contact Meraki support.Learn
more here.
Design A - Cloud Spokes and Cloud Hubs
This design supports both Secure Internet Access (SIA) and Secure Private Access (SPA) use cases. It provides a straightforward, cloud‑based SD‑WAN
transport model in which all MX networks operate as Cloud Spokes.
All customer branch spokes are enrolled in Cisco Secure Access, and no MX hubs are deployed. All traffic-spoke‑to‑spoke, remote worker to site, and endpoint
to IPSec‑attached applications-flows through the cloud path. Because MX hubs are removed from the design, it can scale to a very large number of sites,
making it particularly suitable for environments such as retail deployments.
please ensure to contact support to intervene after onboarding and again disabletheMX Hub mesh.
Design C is the only scenario that supports disabled MX Hub mesh. If necessary, you can do so in the
maintenance window.
UsetheCall Me Nowfeature in the Meraki dashboard to request an immediate phone call.
Phone calls guarantee faster response times and can deploy these changes during your maintenance window.
5

Is this Design for me? - Customer Traffic Profile
Organizations whose traffic is primarily north‑south toward the internet align well with this architecture. East‑west traffic is also carried through the cloud fabric
and inspected by the cloud firewall, consolidating traffic into a single security and routing domain.
Organizations with 500 or more sites within a region can benefit significantly from this approach by reducing the number of hub connections required and
minimizing route scale at spokes. This design also appeals to customers who prefer not to purchase, host, or maintain hub hardware.
All MX appliances function as Cloud Spokes connected to the cloud fabric. Remote workers can reach any spoke‑advertised prefixes through the cloud. Each
Spoke can select the nearest Cloud Hub as its primary, and all Cloud Hubs automatically form a mesh within Secure Access.
Design A - Pros
• Simplified SD‑WAN architecture with minimal configuration
• Supports both SIA and SPA use cases
• Small routing tables at spokes, with the cloud fabric providing full prefix reachability
• No MX hub hardware to deploy or maintain
• Cloud firewall protection for east‑west traffic
• DIA breakout supported, allowing spokes to send trusted SaaS traffic directly to the internet
• Ideal for customers with 500+ sites
• Broad regional coverage through Secure Access integration
6

Design A - Cons
• No support for MX Local Hubs or Hybrid Hubs
• Less flexibility to alter traffic paths
• No customer‑owned hubs to provide backup for cloud transport-traffic relies entirely on the cloud fabric as the backbone
Design B - Any Spokes with Hybrid and Cloud Hubs
This design supports both Secure Internet Access (SIA) and Secure Private Access (SPA) use cases. It allows a mix of Cloud Spokes, Hybrid Spokes, and Local
Spokes, creating a flexible architecture that can adapt to varying connectivity and application access needs.
In the SPA use case, Cloud Spokes use the cloud path for application access, while Hybrid and Local Spokes can use the MX path to reach applications directly
through their enrolled MX hubs. In private access scenarios, an MX‑based traffic path (dotted green) is used byHybrid Spokes to access Hybrid Hub resources.
For internet‑bound traffic, Secure Internet Access (SIA) is used for most sites. Even Hybrid Hubs can enable a default route via SSE to send internet traffic
through the cloud path for SIA.
7

Is this Design for me? - Customer Traffic Profile
This design supports a broader range of traffic patterns by leveraging both the Meraki SD‑WAN fabric and the cloud fabric. Customers can selectively route
east‑west traffic through the Meraki SD‑WAN fabric when needed, bypassing the cloud fabric. This flexibility increases capability but also introduces more
complexity compared to simpler designs.
All traffic directed into the cloud fabric is subject to a unified cloud firewall policy. East‑west flows-such as spoke‑to‑hub, remote‑worker‑to‑SD‑WAN, and
spoke‑to‑spoke-can be governed by this common security layer. When Hybrid Spokes communicate directly with Hybrid Hubs through the MX fabric, these flows
bypass the cloud firewall.
Cloud Spokes can still be used for the majority of branch sites, maintaining simple routing and configuration. Hybrid Spokes provide additional flexibility by
enabling direct communication to selected Hybrid Hubs when performance or latency advantages exist. Remote workers (RAVPN or ZTNA) must be explicitly
permitted in policy to access SD‑WAN resources. Applications behind SD‑WAN must also be defined and allowed as policy destinations.
Resources connected through the cloud transport can communicate freely, except when Hybrid Spokes are configured with priority MX Hubs. Assigning a Hybrid
Hub in the hub‑priority list ensures direct MX‑tunnel connectivity from that Hybrid Spoke.
Design B - Pros
• Supports both SIA and SPA use cases
• Spoke‑to‑spoke traffic can traverse the cloud with a common security policy
• Hybrid Spokes allow direct spoke‑to‑hub access through the MX fabric to improve performance or latency
• Cloud Spokes maintain simple configuration while providing full access to cloud resources
• Provides a balanced approach between security and direct connectivity
8

• Faster convergence improves onboarding speed and redundancy
• Additional redundancy from Local and Enrolled Hubs ensures east‑west traffic continues even if the cloud path
experiences issues
Design B - Cons
• Requires additional MX hardware for Local and Enrolled Hubs
• Dual‑backbone routing increases complexity and troubleshooting difficulty
• Routing defaults to the cloud path, with direct MX‑based paths used only when configured on Hybrid Spokes
• Hybrid Spoke-to-Enrolled Hub traffic bypasses the cloud firewall
Design C: Any Spokes with Local and Cloud Hubs
This design aligns best with the organizations that purchase only theSecure Internet Access (SIA) package. It is centered on maintaining the traditional SIG
(Secure Internet Gateway) model, where the MX SD‑WAN fabric handles east‑west traffic. Itpreserves high‑performance interconnectivity over MX WAN links
while avoiding cloud‑based inspection for private traffic. In this model, Hybrid Spokes list both cloud and MX hubs, with MX hubs prioritized over cloud transport.
9

Is this Design for me? - Customer Traffic Profile
This design provides SIA protection for enrolled Hybrid Spokes with direct MX transport to maintain the performance benefits of the Local Fabric. Furthermore,
this architecture is future-proof; organizations can transition tocloud-based east-west transport after adding anSPA userlicense. Cloud Spokes are excluded
from this design as they lack local fabric connectivity.
Configuration and Connectivity
To achieve optimal connectivity for Spoke-to-Hub application access, configure the relevant Local Hubs on all Hybrid Spokes.
• Local Hub Reachability:ALocal Spoke can communicate with applications hosted on the configured MX Hub;
however, it cannot reach CloudSpokes or endpoints enrolled in the SSE.
Spoke-to-Spoke Communication
When you enableSpoke-to-Spoke communication via a Local MX Hub, it impacts the routing behavior of all Spokes, as traffic paths will prioritize local MX
Hubs.
• Fabric Compatibility: Hybrid and Local Spokes that share the same group of MX Hubs can communicate
effectively. Maintainingaconsistent group of local Hubs across Hybrid Spokes ensuresadirect path of connectivity.
• Cloud Limitations: Cloud Spokes are not part of the Local Fabric and, therefore, cannot communicate with other
Spokes in this configuration.
Remote Access (RAVPN and ZTNA)
Remote workers using RAVPN or ZTNA have specific limitations in this design:
• Local Hubs: Remote workers cannot access subnets located behind Local Hubs.
• Hybrid Spokes: Networks behind Hybrid Spokes are treated as cloud-enrolled resources and are fully reachable by
remote workers.
10

Transport Prioritization
Resources connected via cloud transport maintain full communication protected by the cloud firewall. Hybrid Spokes always prioritize a direct path to Local Hubs
to reach those applications and do not use the Cloud path. Reaching other Spokes with "Spoke-to-Spoke via MX Hub" isensured by sharingthe same group of
configured Local Hubs.
Design C - Pros
• Simple deployment for customers focused exclusively on the SIA use case
• Spoke‑to‑hub and spoke‑to‑spoke traffic stays on the MX transport for full MX platform performance
• Familiar SIG‑style architecture with MX transport preferred over cloud transport
Design C - Cons
• Spoke-to-spoke and spoke-to-hub traffic is not inspected by the cloud firewall
• Hybrid Spokes must maintain consistent Hub priority lists
Support for additionalMeraki designs isbeing validated now thatMeraki with Secure Access solution reachedGeneral Availability.
Design D: Hybrid Hubs with Cloud Hubs (No Spokes)
This design is ideal for organizations that prioritize ahub mesh local path for all site-to-site communication, while gaining the benefit of Secure Private Access
(SPA) for remote workers into Hybrid Hub applications. It offers the flexibility to enable a default route on any hub to support Secure Internet Access (SIA) use
cases. This architecture preserves high-performance interconnectivity over Meraki SD-WAN while exempting private site-to-site traffic from cloud-basedsecurity
services inspection.
11

Is this Design for me? – Customer Traffic Profile
• Remote Access: Provides SPA protection for remote workers accessing enrolled Hybrid Hubs.
• Performance: Hub-to-Hub traffic utilizes direct MX transport (local path) to maintain the full performance benefits of
the Local Fabric.
• Future-Proofing: Organizations can seamlessly add SIA transport in the future by obtaining SIA user licenses and
enabling a default route on the Hybrid Hubs.
Hub-to-Hub Communication
By default,hub mesh is enabled within the Meraki organization. This allows all MX Hubs to establish AutoVPN tunnels and iBGP peering to exchange routes
automatically. This direct, site-to-site communication ensures the lowest latency and highest performance for inter-site traffic.
Remote Access (RAVPN and ZTNA)
Remote workers connecting via RAVPN or ZTNA gain secure access to all Hybrid Hub applications. Their traffic travels through secure tunnels and reaches the
destination applications via AutoVPN on the back end.
Scale of Design D
Cisco has validated this design to a scale of100 Hybrid Hubs (100 MX Hubs enrolled in Secure Access via AutoVPN), with each MX hub managing90
prefixes.
• Route Density: This scenario drives approximately 9,000 routes per hub, leaving adequate headroom for stable
operations.
• Note: Always keep the10,000-route limit of the MX appliance in mind when adding additional routes from other
Secure Access enrolled entities.
Design D – Pros
Pros
• Simplicity: A streamlined deployment for customers focused exclusively on the SPA hub mesh use case.
• Peak Performance: Leverages native MX transport for the best possible site-to-site throughput.
• Familiarity: Utilizes the standard Meraki Hub mesh architecture with the flexibility of an optional SIA cloud path for
Internet-bound traffic.
Design D – Cons
Cons
• Limited Inspection: Hub-to-Hub traffic is not inspected by cloud security services.
12