ISP Architecture – MPLS Overview, Design 
and Implementation for WISPs.  
 
KEVIN MYERS , NETWORK ARCHITECT / 
MANAGING PARTNER  
 
MTCINE #1409  
 
MIKROTIK CERTIFIED TRAINER  
 
 
www.iparchitechs.com 
 1-855-MIKROTI(K)

•Kevin Myers, Network Architect 
• Jackson, Mississippi – United States  
• 18 + years in IT, Network Architecture and Engineering 
 
• Areas of Design Focus: 
• MikroTik integration with large multi-vendor networks  
• Design/Implement/Operate BGP/MPLS/OSPF Wireline 
and WISP service provider networks 
• Design/Implement/Operate Data Center (Enterprise 
and Cloud) networks 
• Certifications 
• MTCINE #1409 & MikroTik Certified Trainer  
• MikroTik – MTCWE, MTCUME, MTCRE, MTCTCE, MTCNA 
• Cisco/Microsoft  – CCNP , CCNA, MCP

• www.iparchitechs.com 
• Global Leaders in MikroTik Design and Engineering 
• #1 ranked MikroTik consulting firm in North America  
• The most successful MikroTik global integrator – we bill 
thousands of hours in MikroTik engineering across 6 
continents.  
• The first consulting firm to offer 24/7 MikroTik 
technical assistance with enterprise level SLAs 
• Operate at large scale supporting networks with tens of 
thousands or routers, switches, firewalls, etc

• www.iparchitechs.com 
• Our Services 
• Global Professional Services – Consulting for Design, 
Engineering, Integration and Operations  
• Fully Managed Network Services -  per rack unit 
support for full network management and monitoring 
• 24/7 support contracts per device – support all 
MikroTik devices with 24/7 TAC support and 4 hour 
SLAs. 
• MultiLingual Support in: English, Français, Polski, Español

• Theory: Briefly introduce the MPLS protocol and how it 
works in conjunction with existing L2/L3 networks 
 
• Design: Discuss an MPLS architecture and preparing 
your WISP for implementing MPLS. 
 
• Business Justification: Identify the business and 
financial use case for implementing MPLS in a WISP .  
 
• Build: Review and discuss examples of MPLS use cases 
and implementation in a WISP . 
 
MPLS – What is it?

What problem are we trying to solve? 
• Isolation of traffic  – MPLS allows a network operator to isolate traffic 
between customers or segments of the WISP network. It also allows for 
the use of overlapping IP space which is useful when building private 
networks for customers. It is also useful when logical separation is 
needed for industry compliance standards like PCI or HIPAA.  
 
• Transporting other protocols over MPLS – MPLS will transport a number 
of protocols by encapsulating it inside of an MPLS packet. Some of the 
more popular uses are: Ethernet, ATM and T1 (PPP and HDLC).  Ethernet 
is probably the most popular and allows an L2 frame to be sent over an L3 
network which creates a PtMP L2 domain over long distances if needed. 
 
• Reduce Complexity/Increase Flexibility – Although MPLS does add 
complexity to the network, in the long run, it reduces complexity by 
allowing complex network problems to be solved in a practical and 
scalable way without a number of “one off” solutions. Being able to 
deploy Layer 2 or 3 overlays and isolation anywhere in the WISP make 
MPLS the Swiss Army Knife of protocols.

MPLS header – The Layer 2.5 protocol 
 
• Example  of  an IPv4 packet – MPLS is commonly referred to as the 
layer 2.5 protocol because the MPLS header sits right between the 
Layer 2 and Layer 3 headers. This is also why MPLS networks 
require more MTU at Layer 2 so that MPLS labels can exist and a 
minimum of a 1500 byte packet can still be handed off. 
 
• Image source: blog.ine.com

MPLS Label Distribution Protocol – Assigning Labels 
 
• Label Distribution Protocol  (LDP) – Similar to a routing protocol, 
LDP maintains the database of MPLS labels and exchanges labels 
with other LDP neighbors.  LDP relies on the underlying routing 
information provided by an IGP (OSPF, IS-IS, EIGRP) in order to 
forward label packets.

• MPLS Forwarding – Routing FIB vs. MPLS FIB (or LFIB) 
• MikroTik Routing Forwarding Information Base (FIB)  –  When a 
Non-MPLS routed network needs to forward a packet, it does so via 
the FIB which is usually a mirror of the routing table.  
Image source: blog.ipspace.net

• MPLS Forwarding – Routing FIB vs. MPLS FIB (or LFIB) 
• MPLS (Label) Forwarding Information Base (LFIB/FIB generic)  –  
When an MPLS network needs to forward a packet, it does so via the 
MPLS FIB or LFIB which is drawn from the Routing table or Routing 
FIB depending on the vendor.

MPLS Architecture – PE, P and CE routers

MPLS Architecture – PE, P and CE routers 
• Provider Edge (PE) router  –  also known as an Ingress/Egress Label 
Switch Router (LSR), is a router between one network service provider's 
area and areas administered by other network providers/ customers.  
 
• Provider (P) router  –  is a Label Switch Router (LSR) that functions as a 
transit router of the core network. The P Router is typically connected to 
one or more PE Routers. 
 
• Customer Edge (CE) router – The customer edge (CE) is the router at the 
customer premises that is connected to the provider edge of a service 
provider IP/MPLS network. CE peers with the Provider Edge (PE) and 
exchanges routes with the corresponding VRF inside the PE. The routing 
protocol used could be static or dynamic (an interior gateway protocol 
like OSPF or an exterior gateway protocol like BGP).

• MPLS Architecture – MTU in the radios, copper and fiber 
• MTU sizing is the most common MPLS mistake –  When building 
MPLS for a WISP , getting the minimum MTU to be standardized and 
supported is the most common mistake we see in real world 
operations. Many WISPs want to reuse radios that can’t support 
MPLS minimum MTUs and end up wasting time/money in 
troubleshooting. 
 
MPLS Basic  MPLS Untagged 
VPN 
MPLS Tagged VPN 
L2 MTU - 1522 L2 MTU - 1526 L2 MTU - 1530 
L3 MTU - 1500 L3 MTU - 1500 L3 MTU - 1500 
MPLS MTU - 1508 MPLS MTU - 1526 MPLS MTU – 1530

Preparing your WISP to support MPLS 
• CPU - Ensure that routers have sufficient CPU capacity for the control 
plane –  While MPLS was originally developed as a way to make routers 
more efficient, in today’s MPLS network, more CPU is desirable to support 
a wide array of services and features as well as increased routing table 
size and number of routing adjacencies.   
 
• Memory –  If you have a need to take in large private or public routing 
tables, ensure the memory in your routers is sufficient. Memory 
consumption varies a bit by vendor for VRF usage and routing tables. 
 
• QoS – MPLS changes QoS architectures as it has only a 3 bit field (EXP) to 
carry traffic markings vs an 8 bit DSCP field. Also, some network devices 
can’t see a DSCP value once an MPLS label is applied so planning EXP to 
DSCP mapping is important as well as where and how to mark/shape 
traffic. 
 
• MTU – In addition to radios and routers, it is important to ensure all 
switches are capable of supporting the minimum MPLS MTU for your use 
case.

Using BGP/OSPF to support MPLS  
• BGP/OSPF – OSPF builds the topology of the network and provides 
loopback and next hop reachability for BGP and MPLS. BGP then 
utilizes MPLS to advertise customer VRF subnets and maintain 
logically separate routing tables in the VPNv4 community.

Implement MPLS carefully  
• Build a lab for MPLS based on production – This can be virtual or physical, but 
it’s important to understand how MPLS will behave on your existing config and 
what changes will occur. The lab can be based on all or part of the existing 
network. 
 
• Implement on a low priority segment –  The very worst thing you can do is 
implement a complex protocol like MPLS for the first time using your highest 
priority subscribers as Guinea pigs. Pick a low impact segment of the network 
to attempt to take your lab into production. For a WISP this can be as simple as 
one tower router and one core router.  
 
• Don’t be afraid to use new hardware – We often see WISPs go to great lengths 
to try and convert an existing production router into an MPLS capable router. 
Sometimes this is feasible and sometimes it’s actually less expensive to build 
MPLS for a small segment with new gear. Once everything is running and 
stable, a migration can be planned for the rest of the network. Adding a new 
tower can be a great opportunity to bring an MPLS trial online since new 
hardware is already required.

Business case for MPLS – part 1  
• Increase revenue using existing infrastructure – MPLS enabled connections 
will sell for much more than an basic Internet pipe due to the provider offering 
managed routing and QoS. It is not uncommon for an MPLS circuit to sell for 4 
times the cost of a non MPLS circuit at the same speeds.

Business case for MPLS – part 2  
• Lower OPEX cost by increasing agility – MPLS enabled networks are more agile and can 
solve complex problems that a customer may require more efficiently than a non MPLS 
network.  
 
• Build private customer networks outside your service area – If a business in your 
footprint has several locations in your service area, but one or more locations outside of 
the service area, you can still capture that sale and manage the entire private WAN 
infrastructure for that company. MPLS can be used to facilitate this. 
• Use MPLS over tunnels to extend the VRF over the Internet. 
• Purchase transit from the last mile provider and use MPLS to connect all the 
locations.  
• Manage the entire solution for a premium fee.  
• Customer gets a Tier 1 business class product with more personalized service. 
 
• Resell your network to Tier 1 and 2 providers – with MPLS enabled, you can quickly 
hand off last mile Layer 2 or 3 circuits to other providers for redundancy or primary 
transit. 
 
• Transport legacy technologies – MPLS can encapsulate legacy technologies like ATM, 
Frame Relay and PPP and deliver them with a much lower cost than traditional legacy 
end to end service.

MPLS WISP Use case #1 – PPPoE aggregation with 
VPLS  
• Problem – Need multiple L2 domains aggregated over a routed network for PPPoE 
• Solution – Use VPLS to extend L2 from the BRAS to the last mile.

MPLS WISP Use case #2 – BGP Peering with a full 
table  
• Problem – Need to sell public BGP transit to multiple customers without impacting BGP edge  
performance 
• Solution – Use VPLS to hand off a /30 that directly connects the CE and PE routers without the need  
for a full table in the core

MPLS WISP Use case #3 – Private L3 Transit (L3VPN)  
• Problem – Need to sell private transit to multiple customers and keep them isolated 
• Solution – Use L3VPN to build private VRF segment for each customer.

MPLS WISP Use case #4 – Private L2 Transit (L2VPN)  
  
• Problem – Need to sell private L2 transit to multiple customers over an L3 networks 
• Solution – Use L2VPN to build private L2 pseudowire for each customer.

Questions? 
The content of this presentation will be available at 
mum.mikrotik.com 
 
Please come see us at the IP ArchiTechs booth in the Exhibitor Hall 
 
Email: kevin.myers@iparchitechs.com 
Office: (303) 590-9943 
Web: www.iparchitechs.com 
 
Thank you for your time and enjoy the MUM!!